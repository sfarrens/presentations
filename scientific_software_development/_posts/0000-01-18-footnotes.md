# Footnotes

--

> These slides contain footnotes to add further context to some points raised in previous slides.

--

> $1$: This is used to demonstrate modern packaging and dependency management. The same exercises could be done using other tools such as `pip` + `venv`, [Poetry](https://python-poetry.org/), or [Conda](https://docs.conda.io/). `uv` was chosen here for its speed and because it consolidates environment, dependency, and Python version management into a single tool.

[Return to slide](#/1/2)

--

> $2$: Breaking down the `uv init` arguments used here:

* ``--lib``: Scaffolds an installable *library* package, using the `src/` layout.
* ``--name mycosmo``: Sets the package name explicitly, since `uv` would otherwise infer it from the directory name (`example`).
* ``-p 3.12`` (short for ``--python``): Pins the Python version in `.python-version` and `pyproject.toml`.
* ``.``: Initialises the project in the current directory, rather than creating a new one.

[Return to slide](#/3/4)

--

> $3$: What `uv init` created:

* ``pyproject.toml``: The configuration file, containing package metadata, dependencies, and tool configuration.
* ``src/``: The package source, containing the actual code.
* ``.python-version``: The pinned Python version so `uv` always selects the same interpreter for this project.
* ``.gitignore``: Standard ignore rules so common Python artifacts never get tracked.
* ``README.md``: A placeholder project readme.

[Return to slide](#/3/5)

--

> $4$: It is good practice to avoid hard-coding parameters inside functions. In this particular example, the cosmological constants in question will likely be needed for other functions and we don't ever want to copy and paste code. Therefore, here we want to abstract the constants such that they can be provided as arguments. An alternative approach would be to add the function to a class and make these constants class attributes.

[Return to slide](#/3/13)

--

> $5$: Branch naming convention: prefix by purpose (`feature/`, `bugfix/`, `hotfix/`, `chore/`, `refactor/`) and use kebab-case (lowercase, dash-separated) for the description, e.g. `feature/critical-density`.

[Return to slide](#/3/17)

--

> $6$: Pro tip 😎: You can combine the two previous commands using the `-b` option for `checkout`. 

```bash
git checkout -b feature/critical-density
```

[Return to slide](#/3/18)

--

> $7$: It is good practice to make regular focused merges with minimal changes to the code. This make things easier to maintain and reduces the chances of breaking something. Therefore, feature branches should only exist long enough to resolve the issue for which they were created. Long-lived feature branches are more likely to evolve into a conflicting state. 

[Return to slide](#/3/25)

--

> $8$: It is good practice for the name of the directory that contains the Python code, `mycosmo` in this case, to match the package name, which we also call `mycosmo`. However, this is not required to make things work.

[Return to slide](#/4/2)

--

> $9$: `uv_build` is `uv`'s own native build backend — lightweight, fast, and set automatically whenever `uv init` scaffolds a new project. It isn't the only option though; other common backends include `setuptools` (the long-standing traditional default), `hatchling` ([Hatch](https://hatch.pypa.io/)'s backend, a popular modern choice), `flit-core` (minimal, aimed at simple pure-Python packages), and `poetry-core` ([Poetry](https://python-poetry.org/)'s backend). Any PEP 517-compliant backend works the same way from the user's perspective — `pip install`/`uv build` just delegate to whichever one is declared here.

[Return to slide](#/4/6)

--

> $10$: `uv.lock` is a fully-resolved, cross-platform lockfile — it pins the *exact* version (and hash) of every dependency, direct and transitive, not just the loose ranges written in `pyproject.toml`. This is what actually guarantees reproducibility: anyone who runs `uv sync` against this repo gets byte-for-byte the same dependency set we have. Unlike `.venv/`, it should be committed to version control, not gitignored — it's regenerated automatically whenever `uv add`, `uv sync`, or `uv lock` runs.

[Return to slide](#/4/8)

--

> $11$: There are some limitations as to what you can do with private repositories on GitHub with a free account, however all features are fully available for public repositories.

[Return to slide](#/5/2)

--

> $12$: The name `origin` is completely arbitrary, but this is the community standard.

[Return to slide](#/5/9)

--

> $13$: The `-u` option is short for `--set-upstream` and is only needed the first time push to a repository that you have initialised locally. In other words, if you create your repository on GitHub or GitLab and clone it, the you won't need to use this option the first time you push.

[Return to slide](#/5/11)

--

> $14$: The `-A` option is short for `--all` and adds all modified or untracked files to the staging area.

[Return to slide](#/5/16)

--

> $15$: Pro tip 😎: You can list both local and remote branches with the `-a` option.

```bash
git branch -a
```

[Return to slide](#/5/20)

--

> $16$: Both GitHub and GitLab have options for automatically deleting feature branches after a PR/MR. Local branches will always have to be managed manually.

[Return to slide](#/5/20)

--

> $17$: Pro tip 😎: You can clean the list of remote branches (see [$15$](#/15/16)) using the `prune` option.

```bash
git remote prune origin
```

[Return to slide](#/5/20)

--

> $18$: `@pytest.mark.parametrize` runs `test_hubble` once per `(redshift, expected)` pair, as an independent test case. This is current best practice over a single test with an array of inputs: if one redshift's calculation breaks, you see exactly which one failed (e.g. `test_hubble[0.5-91.6]`) instead of one opaque array-comparison failure covering all three.

[Return to slide](#/6/4)

--

> $19$: In general, we would like to get as close to 100% coverage as possible. However, just because our coverage report says we are at 100%, it doesn't mean that we have tested everything we can possibly test, or indeed that our tests are any good. A code with lower coverage but with better quality tests may well be more robust.

[Return to slide](#/6/9)

--

> $20$: `git add -A` picks up: the new `tests/test_cosmology.py`, and `pyproject.toml` plus `uv.lock` (both modified by the `uv add --group test ...` commands run earlier). `.coverage`, the data file `pytest-cov` just generated, is skipped since we ignored it above.

[Return to slide](#/6/12)

--

> $21$: `Ruff` implements over 700 lint rules, many of which are re-implementations of rules from older, separate tools (`flake8` and its plugins, `pyupgrade`, `pydocstyle`, and more), alongside its own Black-compatible formatter. Because it's a single native binary rather than a chain of separate Python-based tools, it is typically 10–100x faster, which is the main practical reason it has displaced Black + isort + flake8 as the default choice for new projects.

[Return to slide](#/7/3)

--

> $22$: `ruff linter` lists every rule-code prefix (e.g. `E` = pycodestyle, `F` = Pyflakes, `I` = isort) alongside the tool it corresponds to. To look up what a specific rule actually checks for, `ruff rule <CODE>` explains it in detail — e.g. `ruff rule F401` explains the "unused import" rule. The full, searchable rule reference is also available at [docs.astral.sh/ruff/rules](https://docs.astral.sh/ruff/rules/).

[Return to slide](#/7/5)

--

> $23$: Git keeps a full copy of *every version* of every committed file, forever — so large files (datasets, model weights, build artifacts) permanently bloat the repository, even if deleted in a later commit. If you need to version large files, use [Git LFS](https://git-lfs.com/), which stores them outside the normal Git history.

[Return to slide](#/7/12)

--

> $24$: `git add -A` picks up: `pyproject.toml` and `uv.lock` (modified by the two `uv add --group lint ...` commands), the new `.pre-commit-config.yaml`, and — less obviously — `src/mycosmo/cosmology.py`, since `ruff check --fix .` silently reordered its `from .constants import Mpc, G` line to `G, Mpc` along the way.

[Return to slide](#/7/15)

--

> $25$: [pyright](https://microsoft.github.io/pyright/) is the other widely-used option — often faster, and what VS Code's Pylance extension uses under the hood. Either is a reasonable choice; `mypy` is used here as the longer-established, more conservative default.

[Return to slide](#/8/3)

--

> $26$: `git add -A` picks up: `pyproject.toml` and `uv.lock` (modified by `uv add --group lint mypy`), and `src/mycosmo/cosmology.py` (the new type hints). `mypy` also creates a `.mypy_cache/` directory, but that's harmless to leave out — `mypy` drops its own `.gitignore` inside it automatically, so `git add -A` skips it either way.

[Return to slide](#/8/9)

--

> $27$: Other *README* formats, such as reStructuredText (`README.rst`), are supported by most hosting platforms.

[Return to slide](#/9/3)

--

> $28$: The `sphinx-quickstart` command will only need to be run once. The `--sep` option keeps the source files and the built HTML in separate `docs/source` and `docs/build` directories, which is what the rest of this section assumes.

[Return to slide](#/9/7)

--

> $29$: Breaking down `-Mfeo`: `-M` (`--module-first`) puts module-level documentation before submodule documentation; `-f` (`--force`) overwrites any `.rst` files left over from a previous run; `-e` (`--separate`) puts each module on its own page rather than one combined page. `-o` (`--output-dir`) is the odd one out — it requires a value, so bundled here it consumes the *next* argument (`docs/source`) as the output directory, which is why the module path (`src/mycosmo`) comes after that. Note that `sphinx-apidoc` needs to be re-run any time a module is added to or removed from the package.

[Return to slide](#/9/9)

--

> $30$: Note the `r` prefix — we're going to add some LaTeX to this docstring further down, and backslashes only behave predictably in a *raw* string.

[Return to slide](#/9/13)

--

> $31$: Docstrings need to adhere to reStructuredText formatting standards. The elements with backquotes(` `` `) will be rendered as *literals*.

[Return to slide](#/9/17)

--

> $32$: NumPy's scalar `repr()` changed between major versions (`70.0` in 1.x vs `np.float64(70.0)` in 2.x) — a good example of why doctests shouldn't rely on a library's exact `repr()` output. Wrapping in `float(...)` sidesteps this, since a plain Python float's `repr()` is stable.

[Return to slide](#/9/21)

--

> $33$: The `sphinx-build` option `-b` sets the builder to use, in this case `doctest`. The option `-E` ensures that all files are read for the tests.

[Return to slide](#/9/22)

--

> $34$: Test files are exempted from docstring checks (`"tests/**" = ["D"]`) — this is standard practice, since tests communicate intent through their names and assertions rather than prose, and requiring docstrings there just adds noise. Without this, `tests/test_cosmology.py` from Unit Testing would start failing `ruff check .` from this point on. `docs/**` is exempted too — `docs/source/conf.py` is Sphinx's auto-generated config file, not our own source code, so it shouldn't be held to the same docstring standard.

[Return to slide](#/9/27)

--

> $35$: `git add -A` picks up: `pyproject.toml` and `uv.lock` (from `uv add --group docs ...` and the later `ruff` config change), `src/mycosmo/cosmology.py` (the new docstrings), and the scaffolded `docs/source/conf.py`, `docs/source/index.rst`, `docs/Makefile` and `docs/make.bat`. The built `docs/build/` output is skipped automatically — `uv init`'s default `.gitignore` already has a bare `build/` rule, which git matches at any depth, not just the repo root.

[Return to slide](#/9/29)

--

> $36$: `.github` is a *hidden* directory (its name starts with a dot), so if you're watching `tree` to see files appear, the default `tree` won't show it — you'll need `tree -a` to see hidden files, and probably `tree -a -I '.git'` too, since `-a` also exposes `.git`'s internals.

[Return to slide](#/10/3)

--

> $37$: This specifies that the workflow is called `CI` and that it should only run if we open a pull request to the `main` branch. The `concurrency` block cancels any still-running CI run for the same PR whenever a new commit is pushed — there's no point finishing a check against code that's already been superseded.

[Return to slide](#/10/4)

--

> $38$: `enable-cache: true` caches our resolved packages between runs, so CI doesn't have to re-download the whole dependency set from scratch every single time.

[Return to slide](#/10/5)

--

> $39$: We use `ruff format --check` here, not `ruff format` — CI should *report* problems, never silently rewrite a contributor's code.

[Return to slide](#/10/5)

--

> $40$: If the `fail-fast` option is set to `true`, then all jobs will be aborted if any of the tests fails. This can be useful if you have a large *matrix* of jobs. However, some failures will be system dependent, therefore it can also be useful to set this option to `false` and let jobs run to see on which systems the errors occur.

[Return to slide](#/10/6)

--

> $41$: For reference, here's the complete `ci.yml` file assembled from everything on this slide so far.

```yml
name: CI

on:
  pull_request:
    branches:
     - main

concurrency:
  group: ${{ "{{" }} github.workflow }}-${{ "{{" }} github.ref }}
  cancel-in-progress: true

jobs:
  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v5
        with:
          python-version: "3.12"
          enable-cache: true

      - name: Install dependencies
        run: uv sync --group lint

      - name: Lint
        run: |
          uv run ruff format --check .
          uv run ruff check .
          uv run mypy src

  test-full:
    name: Run CI Tests
    runs-on: ${{ "{{" }} matrix.os }}

    strategy:
      fail-fast: false
      matrix:
        os: [ubuntu-latest, macos-latest]
        python-version: ["3.12"]

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v5
        with:
          python-version: ${{ "{{" }} matrix.python-version }}
          enable-cache: true

      - name: Check Python version
        run: uv run python --version

      - name: Install dependencies
        run: uv sync --group test

      - name: Run tests
        run: uv run pytest
```

[Return to slide](#/10/8)

--

> $42$: This time `git add -A` only picks up one file: the new `.github/workflows/ci.yml`. Unlike previous sections, no dependencies were added here, so `pyproject.toml` and `uv.lock` are untouched.

[Return to slide](#/10/9)

--

> $43$: We will only want to deploy one set of HTML pages. Having API documentation for every branch could be very confusing for the users. Therefore, we only want to trigger our CD workflow on the branch for which we want to deploy the documentation. In this case, the `main` branch.

[Return to slide](#/10/11)

--

> $44$: Unlike the unit tests, we don't care about system dependencies for building our API documentation. This is not something we expect the user to do. So, as long as we can get it to work on one machine, we are good.

[Return to slide](#/10/12)

--

> $45$: The `${{ "{{" }} secrets.GITHUB_TOKEN }}` token is set automatically by GitHub, but you will need to allow write permissions (see [here](#/10/15)).

[Return to slide](#/10/15)

--

> $46$: For reference, here's the complete `cd.yml` file — including the `Checkout`/`Install uv` steps that were described as "the same as the `test-full` CI job" rather than shown again in full.

```yml
name: CD

on:
  push:
    branches:
     - main

jobs:
  docs:
    name: Deploy API Documentation
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v5
        with:
          python-version: "3.12"
          enable-cache: true

      - name: Install dependencies
        run: uv sync --group docs

      - name: Build API documentation
        run: |
          uv run sphinx-apidoc -Mfeo docs/source src/mycosmo
          uv run sphinx-build docs/source docs/build

      - name: Deploy API documentation
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ "{{" }} secrets.GITHUB_TOKEN }}
          publish_dir: docs/build
```

[Return to slide](#/10/15)

--

> $47$: `gh-pages` is a special branch name used by GitHub to identify where HTML content is stored that should be deployed as a website. We don't want to include any other content on the branch. The `--orphan` option for `git checkout` creates a new branch without copying the commit history of the current branch. The `reset` command ensures that the orphan branch is at an initial commit state. The `--allow-empty` option allows us to make a commit without any content. Since `reset --hard` wipes `.pre-commit-config.yaml` along with everything else, `pre-commit`'s hook can no longer find it and refuses to commit at all by default — `PRE_COMMIT_ALLOW_NO_CONFIG=1` tells it that's expected here, rather than disabling checks with `--no-verify`.

[Return to slide](#/10/16)

--

> $48$: On GitHub this is under Settings → Branches → Add branch ruleset → selet Enforcement status "Active", target the "Default" branch, enable "Require status checks to pass before merging" and select the CI and lint jobs. GitLab's equivalent is Settings → Merge requests → Merge checks, or a protected branch pipeline requirement.

[Return to slide](#/10/18)

--

> $49$: Again, just one file: the new `.github/workflows/cd.yml`. The `gh-pages` branch we just created and pushed doesn't factor in here — it's a separate, orphan branch, and we already switched back to `chore/add-ci-cd` before this commit.

[Return to slide](#/10/19)

--

> $50$: `.gitlab-ci.yml` is a special file name that should always be used for GitLab CI/CD.

[Return to slide](#/11/3)

--

> $51$: See [ghcr.io/astral-sh/uv](https://github.com/astral-sh/uv/pkgs/container/uv) for the full list of tags — variants exist for different Python versions and base OS images (e.g. `python3.12-bookworm-slim`, `python3.12-alpine`). Using one of these as the job's `image:` is the GitLab equivalent of the `astral-sh/setup-uv` action on GitHub.

[Return to slide](#/11/4)

--

> $52$: `interruptible: true` lets GitLab cancel this job if a newer pipeline starts for the same merge request — the GitLab equivalent of the `concurrency`/`cancel-in-progress` block we added for GitHub Actions.

[Return to slide](#/11/6)

--

> $53$: The crazy stuff 😵‍💫 after `coverage` simply formats the total coverage score. This just depends on which tool is used to generate the coverage score.

[Return to slide](#/11/7)

--

> $54$: For reference, here's the complete `.gitlab-ci.yml` file assembled from everything on this slide so far (the `pages` job comes later, in its own commit).

```yml
image: ghcr.io/astral-sh/uv:python3.12-bookworm-slim

variables:
  UV_CACHE_DIR: $CI_PROJECT_DIR/.uv-cache

cache:
  key: uv-cache
  paths:
    - .uv-cache/

lint:
  interruptible: true
  script:
    - uv sync --group lint
    - uv run ruff format --check .
    - uv run ruff check .
    - uv run mypy src
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

test:
  interruptible: true
  coverage: '/(?i)total.*? (100(?:\.0+)?\%|[1-9]?\d(?:\.\d+)?\%)$/'
  script:
    - uv sync --group test
    - uv run pytest
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

[Return to slide](#/11/7)

--

> $55$: GitLab Pages specifically looks for a job named `pages` with a `public/` artifact directory — this is why the `sphinx-build` output gets moved into `public/` before the `artifacts` step, and why this job can't be renamed the way `lint`/`test` can.

[Return to slide](#/11/9)

--

> $56$: MIT is a *permissive* licence — anyone can use, modify, and redistribute the code with almost no restrictions. Other common choices include the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0) (adds an explicit patent grant), the [BSD 3-Clause License](https://opensource.org/license/bsd-3-clause) (similar to MIT), and the [GNU GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html) (*copyleft* — derivative works must also be open-sourced under the same licence). [choosealicense.com](https://choosealicense.com/) is a good tool for comparing options.

[Return to slide](#/12/3)

--

> $57$: `uv publish` also supports *trusted publishing* (`--trusted-publishing`), which uses GitHub's OIDC identity to authenticate directly with PyPI — no API token needed at all, and nothing to leak if a CI secret is ever exposed. If you move this step into a GitHub Actions release workflow later, that's the option worth reaching for instead of a stored token.

[Return to slide](#/12/6)

--

> $58$: ⚠️ You should make sure your package name is not already taken before uploading something to PyPI.

[Return to slide](#/12/9)

--

> $59$: ⚠️ You will need to increase your package version each time you upload a new distribution to PyPI.

[Return to slide](#/12/10)

--

> $60$: This is the same guarantee we already covered back in Packaging — `uv.lock` pins the *exact* version and hash of every dependency, direct and transitive. What's worth highlighting here is that this isn't a separate "reproducibility feature" we need to bolt on; it's a side effect of using `uv` the way we already have been throughout the course.

[Return to slide](#/13/2)

--

> $61$: Note that *pinning* (i.e. setting a version with `==`) is not always a good idea. Packages like Numpy tend to put a lot of effort into making their libraries backwards compatible. By pinning a dependency we may make our code incompatible with other packages. This really comes down to the scope of our code, whether this is a stand-alone piece of software that should be used in a dedicated environment (like a pipeline) or something more flexible (like a library) that would be used in conjunction with other packages.

[Return to slide](#/13/9)

--

> $62$: The `FROM` command defines the base Docker image on which to build — Astral's official `uv` image already has Python and `uv` installed, so unlike a Conda-based equivalent there's no `apt-get`/build-tools/shell-activation dance needed at all. The `COPY` command copies the contents of the current working directory (including `uv.lock`) into the build environment. `WORKDIR` sets the default working directory inside the container. `LABEL` labels the image. Each `RUN` command defines a new *layer* in the image — lower layers don't need to be rebuilt if an upper layer changes.

[Return to slide](#/13/13)

--

> $63$: `--frozen` tells `uv` to install *exactly* what's pinned in `uv.lock`, without re-resolving or updating anything — the same reproducibility guarantee `uv.lock` gives us locally, now baked into the image itself.

[Return to slide](#/13/13)

--

> $64$: The Docker `build` option `-t` is short `--tag` and sets a label for the corresponding image. The Docker `run` option `-i` is short for `--interactive` and launches an interactive container. The `-t` option is short for `--tty` and allocates a pseudo-TTY for the container. The trailing `bash` matters too — the `uv` base image has no `ENTRYPOINT`, just a default `CMD` of `uv` with no arguments, so `docker run -it mycosmo` alone just runs bare `uv` (prints its help and exits) rather than dropping you into a shell.

[Return to slide](#/13/15)

--

> $65$: Astropy reports densities in `g/cm^3` by default, while our `critical_density` function returns `kg/m^3`. Multiplying by `1e3` converts between the two ($1\ \mathrm{g/cm^3} = 1000\ \mathrm{kg/m^3}$), so both sides of the comparison are in the same units.

[Return to slide](#/14/7)

--

> $66$: `pytest` itself lives in the `test` group, not `verify` — `astropy` was added there on its own. Locally this is easy to miss, since `pytest` is usually already installed from Unit Testing; a fresh environment (like a CI runner) has no such carryover, and `uv run pytest tests/verify` alone would fail with `Failed to spawn: pytest`.

[Return to slide](#/14/10)

--

> $67$: `git add -A` picks up: `pyproject.toml` and `uv.lock` (from `uv add --group verify astropy` and the `addopts` change), and the new `tests/verify/test_astropy.py`. Running `pytest` also creates a `.pytest_cache/` directory, but like `mypy`, it drops its own `.gitignore` inside automatically, so it's never actually staged.

[Return to slide](#/14/11)

--

> $68$: `memray` is actively maintained and, unlike most memory profilers, can attribute allocations to native (C) stack frames as well as Python ones — important for NumPy-heavy code, where most allocations actually happen inside C, not Python.

[Return to slide](#/15/4)

--

> $69$: Background noise — OS scheduling, cache effects, garbage collection pauses — can only slow a run down, never speed it up, so the fastest observed sample is the closest approximation of the code's true cost. This is why `timeit.repeat`, not a single `timeit.timeit` call, is recommended: it gives you several samples to take the minimum of.

[Return to slide](#/15/6)

--

> $70$: Both tools work by instrumenting Python bytecode execution, so a single NumPy call like `cosmology.hubble(...)` appears as one line or one function call no matter how much C code runs underneath it. They're most useful for finding hot *Python* logic (loops, branching, object creation) — for what's happening inside NumPy itself, a native-aware tool like `memray` (with `native_traces=True`) or `py-spy --native` is needed instead.

[Return to slide](#/15/6)

--

> $71$: `native_traces` defaults to `False`. Without it, every allocation made inside a vectorised NumPy call collapses into a single opaque Python frame (e.g. `hubble`) in the resulting flame graph; with it, `memray` also records the underlying C call stack (e.g. NumPy's ufunc machinery), which is what actually did the allocating.

[Return to slide](#/15/15)

--

> $72$: From a February 2025 post on X: "There's a new kind of coding I call 'vibe coding', where you fully give in to the vibes, embrace exponentials, and forget that the code even exists." ([source](https://x.com/karpathy/status/1886192184808149383))

[Return to slide](#/16/4)

--

> $73$: Studies have found that roughly 20% of AI-generated code samples reference packages that don't actually exist, and when the same prompt is re-run repeatedly, over half of the hallucinated names reappear consistently — predictable enough for attackers to register the names in advance. ([source](https://labs.cloudsecurityalliance.org/research/csa-research-note-slopsquatting-ai-supply-chain-20260419-csa/))

[Return to slide](#/16/8)

--

> $74$: Approaches range from projects that reject AI-generated contributions outright, to others that allow them provided they're disclosed via a commit trailer (e.g. `Assisted-By:` for light use, `Generated-By:` for substantial AI-generated patches). A maintained list of real project policies is available [here](https://github.com/melissawm/open-source-ai-contribution-policies).

[Return to slide](#/16/12)

--

> $75$: As of 2026, `AGENTS.md` is read natively by 30+ tools (including Claude Code, GitHub Copilot, Cursor, Codex and Gemini CLI) across more than 60,000 repositories, and is stewarded by the Agentic AI Foundation under the Linux Foundation.

[Return to slide](#/16/12)
