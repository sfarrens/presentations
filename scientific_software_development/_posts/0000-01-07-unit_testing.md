## Unit testing

--

> Once we have some working code, it is a very good idea to write some accompanying *unit tests*. This ensures that individual components of our code (e.g. functions, classes, etc.) do what we expect them to do. This also helps us avoid introducing bugs into the code, as the tests should fail if we break something.

--

> Following the [Git workflow](#/5) we established previously, let's start by creating a feature branch for this work.

```bash
git checkout -b feature/unit-tests
```

--

> Let's write a test for our `cosmology.py` module. We will start by creating a directory called `tests` at the root of the repository.

```bash
mkdir tests
touch tests/test_cosmology.py
```

--

> and paste in the following content.[$^{18}$](#/17/19)

```python
import pytest

from mycosmo.cosmology import hubble

FID_COSMO = {
    "H0": 70,
    "omega_m_0": 0.3,
    "omega_k_0": 0.0,
    "omega_lambda_0": 0.7,
}


@pytest.mark.parametrize(
    "redshift, expected",
    [
        (0.0, 70),
        (0.5, 91.60),
        (1.0, 123.24),
    ],
)
def test_hubble(redshift, expected):
    assert hubble(redshift, FID_COSMO) == pytest.approx(expected, abs=0.01)
```

--

> Before we can run our tests, we need `pytest` itself. Since this is only needed for development, not by anyone installing our package, we add it to a dependency *group* rather than as a regular dependency.

```bash
uv add --group test pytest
```

--

> We can now use the [pytest](https://docs.pytest.org/) package to run the unit tests, via `uv run`.

```bash
uv run pytest --verbose tests
```

> If all went well, you should see three `test_hubble[...]` cases, all *PASSED*.

--

> Now try introducing a deliberate bug into the code. For example, try making the following change to the `hubble` function in `cosmology.py`*.

```python
matter = cosmo_dict["omega_m_0"] * (1 + redshift) ** 4
```

> Now re-run `pytest`. You should see that all three `test_hubble[...]` cases have *FAILED*, along with an explanation of why.

> *Don't forget to revert the changes afterwards!
<!-- .element: style="font-size: 50%;" -->

--

> There are various pytest plug-ins that can help us improve our workflow. Some good examples are:

- [pytest-cov](https://github.com/pytest-dev/pytest-cov): to provide a *coverage* report
- [pytest-emoji](https://github.com/hackebrot/pytest-emoji): if you like emojis 😂

> We can add these to our existing `test` dependency group — `uv` merges them in alongside `pytest` rather than replacing the group.

```bash
uv add --group test pytest-cov pytest-emoji
```

--

> We can invoke these extra features using the corresponding command line options.

```bash
uv run pytest --verbose --emoji --cov=mycosmo tests
```

> The coverage report tells us which fraction of the code has been covered by unit tests.[$^{19}$](#/17/20)

--

> Rather than typing these options every time, we can set them as the *default* `pytest` options in `pyproject.toml`, so `uv run pytest` alone does the same thing.

```toml
[tool.pytest.ini_options]
addopts = ["--verbose", "--emoji", "--cov=mycosmo"]
testpaths = ["tests"]
```

> From now on, we can simply run

```bash
uv run pytest
```

--

> This leaves us with a `.coverage` file in the repository root — `uv`'s default `.gitignore` doesn't cover it. Let's ignore it before committing.

```bash
echo -e "\n# Test results\n.coverage" >> .gitignore
```

--

> Make sure all the tests are passing and then add, commit and push all of the changes to the feature branch.[$^{20}$](#/17/21)

```bash
git add -A
git commit -m "Add unit tests for cosmology module"
git push origin feature/unit-tests
```

> Then open a Pull/Merge request as explained in the [previous section](#/5) before merging the changes and [cleaning up](#/5/20).

--

## Exercise

> Add a new unit test for the `critical_density` function. Use the [Git workflow](#/5/22) that we established previously to implement the updated code via a MR/PR. 

> It is recommended that you work in pairs with each of you acting as the reviewer for each other's MR/PR.
