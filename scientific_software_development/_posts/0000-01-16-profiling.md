## Profiling

--

> Now that our code is tested, documented and easy to maintain, it's worth asking a different question: is it actually *fast enough*? *Profiling* is how we answer that with data instead of guesswork.

> The rule of thumb: **measure before you optimise**. It's easy to guess wrong about where time is going, and "optimising" the wrong part wastes effort without making anything faster.

--

> There isn't one kind of profiling — different tools answer different questions.

|             | Answers                                                      |
| ----------- | ------------------------------------------------------------- |
| Timing      | Is this *overall* fast enough? Did a change make it slower?   |
| Call-graph  | Which *functions* take the most time?                         |
| Line-level  | Which *lines* inside a function take the most time?           |
| Memory      | How much memory is used, and where is it allocated?           |
<!-- .element: style="font-size: 70%;" -->

--

> Each catches a different class of problem. A function might be fast in isolation but called far too often; a single line inside an otherwise-fast function might dominate its runtime; a computation might be quick but memory-hungry enough to crash a job on a shared cluster. No single tool tells you all of this at once.

--

> The Python community has settled on a small set of go-to tools for each of these questions:

- [`timeit`](https://docs.python.org/3/library/timeit.html) (stdlib) — wall-clock timing
- [`cProfile`](https://docs.python.org/3/library/profile.html) (stdlib) — function-level call-graph profiling
- [`line_profiler`](https://github.com/pyutils/line_profiler) — line-by-line timing
- [`memray`](https://bloomberg.github.io/memray/) — memory profiling, with flame graphs[$^{68}$](#/17/69)

--

> A few others worth knowing about, depending on the job:

- [`py-spy`](https://github.com/benfred/py-spy) — a *sampling* profiler; attaches to an already-running process with virtually no overhead, safe to use in production
- [`scalene`](https://github.com/plasma-umass/scalene) — combined CPU/GPU/memory profiler with per-line attribution
- [`pytest-benchmark`](https://pytest-benchmark.readthedocs.io/) / [`asv`](https://asv.readthedocs.io/) — track performance *over time* in CI, rather than a one-off snapshot

--

> A few things worth keeping in mind, regardless of which tool you reach for:

- Use realistic input sizes — a function that's "fast" on 10 elements may behave completely differently on 10 million
- Run more than once and look at the *minimum*, not the mean[$^{69}$](#/17/70)
- For vectorised NumPy code, `cProfile` and `line_profiler` can only see the Python-level call — the real cost is inside NumPy's C internals, invisible to both[$^{70}$](#/17/71)

--

> Let's add a profiling script to `mycosmo` covering all four angles above. Following the [Git workflow](#/5) we established previously, let's start by creating a feature branch for this work.

```bash
git checkout -b chore/add-profiling
```

--

> Since profiling tools are development-only, we add them to their own dependency group.

```bash
uv add --group profile line_profiler memray snakeviz
```

--

> Let's create a `scripts` directory to hold the profiling script, separate from our installable package and our tests.

```bash
mkdir scripts
touch scripts/profile_mycosmo.py
```

--

> Paste in the following content, covering all four profiling angles.

```python
"""Profile Mycosmo.

This script profiles the mycosmo package using timeit, cProfile,
line_profiler and memray.

"""

import cProfile
import pstats
import timeit

import memray
import numpy as np
from line_profiler import LineProfiler

from mycosmo import cosmology

REDSHIFTS = np.linspace(0, 10, 100_000)
COSMO_DICT = {"H0": 70, "omega_m_0": 0.3, "omega_k_0": 0.0, "omega_lambda_0": 0.7}


def profile_with_timeit():
    """Time each function with timeit.repeat."""
    for name, func in (
        ("hubble", cosmology.hubble),
        ("critical_density", cosmology.critical_density),
    ):
        samples = timeit.repeat(
            lambda: func(REDSHIFTS, COSMO_DICT), repeat=5, number=100
        )
        print(f"{name}(): {min(samples) / 100:.6e}s per call")


def profile_with_cprofile():
    """Function-level profiling using cProfile."""
    profiler = cProfile.Profile()
    profiler.enable()
    cosmology.hubble(REDSHIFTS, COSMO_DICT)
    cosmology.critical_density(REDSHIFTS, COSMO_DICT)
    profiler.disable()

    profiler.dump_stats("cprofile_results.prof")
    pstats.Stats(profiler).sort_stats("cumulative").print_stats(10)


def profile_with_line_profiler():
    """Line-by-line profiling using line_profiler."""
    profiler = LineProfiler()
    profiler.add_function(cosmology.hubble)
    profiler.add_function(cosmology.critical_density)

    profiler.enable()
    cosmology.hubble(REDSHIFTS, COSMO_DICT)
    cosmology.critical_density(REDSHIFTS, COSMO_DICT)
    profiler.disable()

    profiler.print_stats()


def profile_memory_usage():
    """Memory profiling using memray."""
    with memray.Tracker("memray_results.bin", native_traces=True):
        cosmology.hubble(REDSHIFTS, COSMO_DICT)
        cosmology.critical_density(REDSHIFTS, COSMO_DICT)


def main():
    """Run all profiling approaches."""
    print("=== Timeit ===")
    profile_with_timeit()

    print("\n=== cProfile ===")
    profile_with_cprofile()

    print("\n=== Line Profiler ===")
    profile_with_line_profiler()

    print("\n=== Memory (memray) ===")
    profile_memory_usage()


if __name__ == "__main__":
    main()
```

--

> `REDSHIFTS` and `COSMO_DICT` are shared inputs, so every profiler exercises the exact same call.

--

> `profile_with_timeit()` times each function with `timeit.repeat`, reporting the *minimum* of several samples.

--

> `profile_with_cprofile()` profiles function-level call counts and cumulative time, dumping a binary `.prof` file that tools like [snakeviz](https://jiffyclub.github.io/snakeviz/) can visualise later.

--

> `profile_with_line_profiler()` breaks each function down line by line, so you can see exactly which line dominates its runtime.

--

> `profile_memory_usage()` tracks memory allocations with `memray`, writing a trace to `memray_results.bin`.[$^{71}$](#/17/72)

--

> `main()` ties the four together, guarded by the usual `if __name__ == "__main__":` entry point. A `print()` header before each step keeps the combined output readable.

--

> Run it the same way as any other script, making sure the `profile` group is synced.

```bash
uv run --group profile scripts/profile_mycosmo.py
```

--

> Visualise the `cProfile` output with `snakeviz`.

```bash
uv run snakeviz cprofile_results.prof
```

--

> And render the memory trace as a flame graph.

```bash
uv run memray flamegraph memray_results.bin
```

--

> This leaves us with a few output files in the repository root — `cprofile_results.prof`, `memray_results.bin`, and `memray-flamegraph-memray_results.html` — none of which are covered by `uv`'s default `.gitignore`. Let's ignore them before committing.

```bash
echo -e "\n# Profiling results\ncprofile_results.prof\nmemray_results.bin\nmemray-flamegraph-memray_results.html" >> .gitignore
```

--

> Let's add, commit and push the changes to the feature branch.

```bash
git add -A
git commit -m "Add profiling script"
git push origin chore/add-profiling
```

> Then open a Pull/Merge request as explained in the [previous section](#/5) before merging the changes and [cleaning up](#/5/20).
