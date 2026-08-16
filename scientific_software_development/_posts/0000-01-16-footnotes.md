# Footnotes

--

> These slides contain footnotes to add further context to some points raised in previous slides.

--

> $1$: This is used to demonstrate modern packaging and dependency management. The same exercises could be done using other tools such as `pip` + `venv`, [Poetry](https://python-poetry.org/), or [Conda](https://docs.conda.io/). `uv` was chosen here for its speed and because it consolidates environment, dependency, and Python version management into a single tool.

[Return to slide](#/1/2)

--

> $2$: Breaking down the `uv init` arguments used here:

* ``--lib``: Scaffolds an installable *library* package (as opposed to a plain application), using the `src/` layout with an `__init__.py` and a `py.typed` marker.
* ``--name mycosmo``: Explicitly sets the package name. Without this, `uv` would infer the name from the containing directory (`example`), which is not what we want.
* ``-p 3.12`` (short for ``--python``): Pins both `.python-version` and the `requires-python` floor in `pyproject.toml` to Python 3.12, so the result is the same regardless of what Python happens to be active on the machine running the command.
* ``.``: Initialises the project in the current directory, rather than creating a new subdirectory.

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

> $12$: The the name `origin` is completely arbitrary, but this is the community standard.

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

> $20$: `git add -A` picks up everything changed in this section: the new `tests/test_cosmology.py`, and `pyproject.toml` plus `uv.lock` (both modified by the `uv add --group test ...` commands run earlier).

[Return to slide](#/6/11)

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

> $34$: Test files are exempted from docstring checks (`"tests/**" = ["D"]`) — this is standard practice, since tests communicate intent through their names and assertions rather than prose, and requiring docstrings there just adds noise. Without this, `tests/test_cosmology.py` from Unit Testing would start failing `ruff check .` from this point on.

[Return to slide](#/9/27)

--

> $35$: `git add -A` picks up: `pyproject.toml` and `uv.lock` (from `uv add --group docs ...` and the later `ruff` config change), `src/mycosmo/cosmology.py` (the new docstrings), and the scaffolded `docs/source/conf.py`, `docs/source/index.rst`, `docs/Makefile` and `docs/make.bat`. The built `docs/build/` output is skipped automatically — `uv init`'s default `.gitignore` already has a bare `build/` rule, which git matches at any depth, not just the repo root.

[Return to slide](#/9/29)

--

> $36$: This specifies that the workflow is called `CI` and that it should only run if we open a pull request to the `main` branch. The `concurrency` block cancels any still-running CI run for the same PR whenever a new commit is pushed — there's no point finishing a check against code that's already been superseded.

[Return to slide](#/10/4)

--

> $37$: `enable-cache: true` caches our resolved packages between runs, so CI doesn't have to re-download the whole dependency set from scratch every single time.

[Return to slide](#/10/5)

--

> $38$: We use `ruff format --check` here, not `ruff format` — CI should *report* problems, never silently rewrite a contributor's code.

[Return to slide](#/10/5)

--

> $39$: If the `fail-fast` option is set to `true`, then all jobs will be aborted if any of the tests fails. This can be useful if you have a large *matrix* of jobs. However, some failures will be system dependent, therefore it can also be useful to set this option to `false` and let jobs run to see on which systems the errors occur.

[Return to slide](#/10/6)

--

> $40$: This time `git add -A` only picks up one file: the new `.github/workflows/ci.yml`. Unlike previous sections, no dependencies were added here, so `pyproject.toml` and `uv.lock` are untouched.

[Return to slide](#/10/9)

--

> $41$: We will only want to deploy one set of HTML pages. Having API documentation for every branch could be very confusing for the users. Therefore, we only want to trigger our CD workflow on the branch for which we want to deploy the documentation. In this case, the `main` branch.

[Return to slide](#/10/11)

--

> $42$: Unlike the unit tests, we don't care about system dependencies for building our API documentation. This is not something we expect the user to do. So, as long as we can get it to work on one machine, we are good.

[Return to slide](#/10/12)

--

> $43$: The `${{ "{{" }} secrets.GITHUB_TOKEN }}` token is set automatically by GitHub, but you will need to allow write permissions (see [here](#/10/15)).

[Return to slide](#/10/15)

--

> $44$: `gh-pages` is a special branch name used by GitHub to identify where HTML content is stored that should be deployed as a website. We don't want to include any other content on the branch. The `--orphan` option for `git checkout` creates a new branch without copying the commit history of the current branch. The `reset` command ensures that the orphan branch is at an initial commit state. The `--allow-empty` option allows us to make a commit without any content.

[Return to slide](#/10/16)

--

> $45$: On GitHub this is under Settings → Branches → Branch protection rules → add a rule for `main` → enable "Require status checks to pass before merging" and select the `CI` job. GitLab's equivalent is Settings → Merge requests → Merge checks, or a protected branch pipeline requirement.

[Return to slide](#/10/18)

--

> $46$: Again, just one file: the new `.github/workflows/cd.yml`. The `gh-pages` branch we just created and pushed doesn't factor in here — it's a separate, orphan branch, and we already switched back to `chore/add-ci-cd` before this commit.

[Return to slide](#/10/19)

--

> $47$: `.gitlab-ci.yml` is a special file name that should always be used for GitLab CI/CD.

[Return to slide](#/11/3)

--

> $48$: See [ghcr.io/astral-sh/uv](https://github.com/astral-sh/uv/pkgs/container/uv) for the full list of tags — variants exist for different Python versions and base OS images (e.g. `python3.12-bookworm-slim`, `python3.12-alpine`). Using one of these as the job's `image:` is the GitLab equivalent of the `astral-sh/setup-uv` action on GitHub.

[Return to slide](#/11/4)

--

> $49$: `interruptible: true` lets GitLab cancel this job if a newer pipeline starts for the same merge request — the GitLab equivalent of the `concurrency`/`cancel-in-progress` block we added for GitHub Actions.

[Return to slide](#/11/6)

--

> $50$: The crazy stuff 😵‍💫 after `coverage` simply formats the total coverage score. This just depends on which tool is used to generate the coverage score.

[Return to slide](#/11/7)

--

> $51$: GitLab Pages specifically looks for a job named `pages` with a `public/` artifact directory — this is why the `sphinx-build` output gets moved into `public/` before the `artifacts` step, and why this job can't be renamed the way `lint`/`test` can.

[Return to slide](#/11/9)

--

> $52$: MIT is a *permissive* licence — anyone can use, modify, and redistribute the code with almost no restrictions. Other common choices include the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0) (adds an explicit patent grant), the [BSD 3-Clause License](https://opensource.org/license/bsd-3-clause) (similar to MIT), and the [GNU GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html) (*copyleft* — derivative works must also be open-sourced under the same licence). [choosealicense.com](https://choosealicense.com/) is a good tool for comparing options.

[Return to slide](#/12/3)

--

> $53$: `uv publish` also supports *trusted publishing* (`--trusted-publishing`), which uses GitHub's OIDC identity to authenticate directly with PyPI — no API token needed at all, and nothing to leak if a CI secret is ever exposed. If you move this step into a GitHub Actions release workflow later, that's the option worth reaching for instead of a stored token.

[Return to slide](#/12/6)

--

> $54$: ⚠️ You should make sure your package name is not already taken before uploading something to PyPI.

[Return to slide](#/12/9)

--

> $55$: ⚠️ You will need to increase your package version each time you upload a new distribution to PyPI.

[Return to slide](#/12/10)

--

> $56$: This is the same guarantee we already covered back in Packaging — `uv.lock` pins the *exact* version and hash of every dependency, direct and transitive. What's worth highlighting here is that this isn't a separate "reproducibility feature" we need to bolt on; it's a side effect of using `uv` the way we already have been throughout the course.

[Return to slide](#/13/2)

--

> $57$: Note that *pinning* (i.e. setting a version with `==`) is not always a good idea. Packages like Numpy tend to put a lot of effort into making their libraries backwards compatible. By pinning a dependency we may make our code incompatible with other packages. This really comes down to the scope of our code, whether this is a stand-alone piece of software that should be used in a dedicated environment (like a pipeline) or something more flexible (like a library) that would be used in conjunction with other packages.

[Return to slide](#/13/9)

--

> $58$: The `FROM` command defines the base Docker image on which to build — Astral's official `uv` image already has Python and `uv` installed, so unlike a Conda-based equivalent there's no `apt-get`/build-tools/shell-activation dance needed at all. The `COPY` command copies the contents of the current working directory (including `uv.lock`) into the build environment. `WORKDIR` sets the default working directory inside the container. `LABEL` labels the image. Each `RUN` command defines a new *layer* in the image — lower layers don't need to be rebuilt if an upper layer changes.

[Return to slide](#/13/13)

--

> $59$: `--frozen` tells `uv` to install *exactly* what's pinned in `uv.lock`, without re-resolving or updating anything — the same reproducibility guarantee `uv.lock` gives us locally, now baked into the image itself.

[Return to slide](#/13/13)

--

> $60$: The Docker `build` option `-t` is short `--tag` and sets a label for the corresponding image. The Docker `run` option `-i` is short for `--interactive` and launches an interactive container. The `-t` option is short for `--tty` and allocates a pseudo-TTY for the container.

[Return to slide](#/13/15)

--

> $61$: Astropy reports densities in `g/cm^3` by default, while our `critical_density` function returns `kg/m^3`. Multiplying by `1e3` converts between the two ($1\ \mathrm{g/cm^3} = 1000\ \mathrm{kg/m^3}$), so both sides of the comparison are in the same units.

[Return to slide](#/14/7)

--

> $62$: `git add -A` picks up: `pyproject.toml` and `uv.lock` (from `uv add --group verify astropy` and the `addopts` change), and the new `tests/verify/test_astropy.py`. Running `pytest` also creates a `.pytest_cache/` directory, but like `mypy`, it drops its own `.gitignore` inside automatically, so it's never actually staged.

[Return to slide](#/14/11)
