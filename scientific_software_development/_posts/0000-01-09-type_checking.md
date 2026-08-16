## Type checking

--

> `uv init --lib` already gave our package a `py.typed` marker, back in [Introduction to Git](#/3/5), which tells other tools our code ships reliable type hints. Right now nothing backs that claim up — let's fix that.

> *Type hints* document what types a function expects and returns; a *type checker* then verifies those hints are actually followed, catching a whole class of bugs before the code ever runs.

--

> Following the [Git workflow](#/5) we established previously, let's start by creating a feature branch for this work.

```bash
git checkout -b chore/add-type-checking
```

--

> [mypy](https://mypy-lang.org/) is the original Python type checker and remains the community standard.[$^{25}$](#/15/26)

--

> Since this is a development-only tool, we add it to our existing `lint` dependency group.

```bash
uv add --group lint mypy
```

--

> Let's add type hints to our `hubble` function.

```python
def hubble(
    redshift: float | np.ndarray, cosmo_dict: dict[str, float]
) -> float | np.ndarray:
```

> and to `critical_density`.

```python
def critical_density(
    redshift: float | np.ndarray, cosmo_dict: dict[str, float]
) -> float | np.ndarray:
```

--

> Just like `pytest` and `ruff`, we can configure `mypy` in `pyproject.toml`.

```toml
[tool.mypy]
python_version = "3.12"
warn_unused_configs = true
disallow_untyped_defs = true
```

> `disallow_untyped_defs` means every function needs type hints — without it, `mypy` silently skips anything that isn't already annotated.

--

> Now we can run `mypy` against our package.

```bash
uv run mypy src
```

> You should see `Success: no issues found`.

--

> Let's confirm this is actually working. Try adding the following anywhere in `cosmology.py`.

```python
answer: int = "42"
```

> Now re-run `mypy`. You should see an *Incompatible types in assignment* error. Remove the bad line, and the check should pass again.

--

> Make sure everything passes, and then add, commit and push all of the changes to the feature branch.[$^{26}$](#/15/27)

```bash
git add -A
git commit -m "Add type hints and mypy"
git push origin chore/add-type-checking
```

> Then open a Pull/Merge request as explained in the [previous section](#/5) before merging the changes.
