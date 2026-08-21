## Version Control and Packaging II
### Packaging

--

> Once our code reaches a certain level of maturity we may want to share it with the rest of the world. You never know who could find it useful. 🙂

> If we would like people to use the code, we should make it easy for them to access and install. This means we need to *package* our software.

> The current community standard for Python is to write a *TOML* file called `pyproject.toml` to package the code and then distribute it via [PyPI](https://pypi.org/) (Python Package Index). This way users can simply install the code using `pip`.

--

> `uv` already created a `pyproject.toml` file for our package, which we have called `mycosmo`.[$^8$](#/17/9) Let's have a look inside.

--

> The first section provides the essential package *metadata*.

```toml
[project]
name = "mycosmo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "<User Name>", email = "<User Email>" }
]
requires-python = ">=3.12"
```

--

> Here we specify the name of the package, what the current version is, what the code does, the `README.md` file, who wrote/maintains the code, and what minimum version of Python is needed to run it.

--

> `uv` has set up most of this metadata for us, let's just modify the description to something more useful.

```toml
description = "This is an example cosmology package."
```

--

> The second section shows the *build system* for the package.

```toml
[build-system]
requires = ["uv_build>=0.12.3,<0.13.0"]
build-backend = "uv_build"
```

> A build system is the tool that turns our source code into a distributable, installable package (e.g. a *wheel*); the *build system*  simply tells installers like `pip` or `uv` which backend to use for that job.[$^9$](#/17/10)

--

> Now is a good time to check that our code actually works. First, we will need to specify what (if any) external dependencies our package uses.

> We can see from the first line of `cosmology.py` 

```python
import numpy as np
```

> that we are using [NumPy](https://numpy.org/), so we can tell `uv` to add this.

```bash
uv add numpy
```

--

> You will notice that the `pyproject.toml` metadata was modified to include the following

```toml
dependencies = [
    "numpy>=2.5.2",
]
``` 

> and a new `uv.lock` file was created.[$^{10}$](#/17/11) Let's add and commit these changes.

```bash
git add pyproject.toml uv.lock 
git commit -m "Add NumPy dependency"
```

--

> With our `pyproject.toml` file ready, we can now install the `mycosmo` package to make sure everything is working as expected.

```bash
pip install .
```

> We can check the metadata with the `show` option.

```bash
pip show mycosmo
```

--

> Let's do a quick check to make sure everything is working as expected.

```bash
python -c "from mycosmo.cosmology import hubble; print(hubble(0.0, {'H0': 70, 'omega_m_0': 0.3, 'omega_k_0': 0.0, 'omega_lambda_0': 0.7}))"
```

> You should get `70.0` as the output.