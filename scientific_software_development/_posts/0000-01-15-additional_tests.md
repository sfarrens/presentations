## Additional tests

--

> While unit tests are essential, there are other types of tests we can write to ensure our code works correctly. One important type is *verification tests* that compare our code against well-established libraries or analytical solutions.

--

> Following the [Git workflow](#/5) we established previously, let's start by creating a feature branch for this work.

```bash
git checkout -b feature/verification-tests
```

--

> Let's verify our cosmological calculations against those provided by the [Astropy](https://www.astropy.org/) library. This is particularly useful because Astropy is a well-tested and widely-used package in the astronomical community.

> Verification tests like this tend to be heavier than unit tests (bigger dependency, slower), so rather than our existing `test` group, we give them a dedicated group of their own.

```bash
uv add --group verify astropy
```

--

> Let's also keep these tests in their own `tests/verify` directory, separate from our regular unit tests.

```bash
mkdir tests/verify
touch tests/verify/test_astropy.py
```

--

> and paste in the following content, comparing our `hubble` function against Astropy's WMAP9 cosmology:

```python
import pytest
from astropy.cosmology import WMAP9 as cosmo

from mycosmo.cosmology import critical_density, hubble

REDSHIFTS = [0.0, 0.5, 1.0]

COSMO_DICT = {
    "H0": cosmo.H0.value,
    "omega_m_0": cosmo.Om0,
    "omega_k_0": cosmo.Ok0,
    "omega_lambda_0": cosmo.Ode0,
}


class TestAstropy:
    """Test Astropy.

    Class to test ``mycosmo`` routines with respect to those provided in Astropy.
    """

    @pytest.mark.parametrize("redshift", REDSHIFTS)
    def test_hubble(self, redshift):
        """Test Hubble function."""
        h_mycosmo = hubble(redshift=redshift, cosmo_dict=COSMO_DICT)
        h_astropy = cosmo.H(redshift).value

        assert h_mycosmo == pytest.approx(h_astropy, abs=0.1)
```

--

> This test:

- Uses Astropy's WMAP9 cosmology as a reference
- Compares our Hubble function calculations at different redshifts, via `@pytest.mark.parametrize`, exactly as we did for our original unit test
- Ensures the differences are within acceptable tolerances

--

> We can also verify our critical density calculations.[$^{65}$](#/17/66)

```python
    @pytest.mark.parametrize("redshift", REDSHIFTS)
    def test_critical_density(self, redshift):
        """Test Critical Density function."""
        rho_crit_mycosmo = critical_density(redshift=redshift, cosmo_dict=COSMO_DICT)
        rho_crit_astropy = cosmo.critical_density(redshift).value * 1e3

        assert rho_crit_mycosmo == pytest.approx(rho_crit_astropy, rel=1e-2)
```

--

> These verification tests provide several benefits:

- They validate our code against a trusted reference implementation
- They help catch numerical precision issues
- They ensure our code produces physically meaningful results
- They make our code more trustworthy for scientific applications

--

> Since these types of tests can be slower and pull in a extra dependencies, let's exclude `tests/verify` from our default `pytest` run, so `uv run pytest` alone still only runs our fast unit tests.

```toml
[tool.pytest.ini_options]
addopts = ["--verbose", "--emoji", "--cov=mycosmo", "--ignore=tests/verify"]
testpaths = ["tests"]
```

--

> We can still run the verification tests explicitly, by pointing `pytest` directly at that directory. Since `astropy` lives in a separate `verify` group from `pytest` itself, we need both groups synced.[$^{66}$](#/17/67)

```bash
uv run --group test --group verify pytest tests/verify
```

--

> Let's add, commit and push the changes to the feature branch.[$^{67}$](#/17/68)

```bash
git add -A
git commit -m "Add verification tests against Astropy"
git push origin feature/verification-tests
```

> Then open a Pull/Merge request as explained in the [previous section](#/5) before merging the changes and [cleaning up](#/5/20).

--

> Other types of tests you might consider adding:

- Integration tests that verify multiple components work together
- Performance tests to ensure code runs efficiently
- Regression tests to catch previously fixed bugs
- Property-based tests that verify mathematical properties

--

## Exercise

> Write a workflow (e.g. GitHub Action) that runs `uv sync --group test --group verify` followed by `uv run pytest tests/verify`, triggered manually via `on: workflow_dispatch` instead of on every push or pull request.
