## Documentation

--

> Now that we have a stable and tested code, we need to make sure users understand how to run it and developers understand how to contribute to it.

> There are two main places this needs to be done: a `README.md` file and *API* documentation.

--

> The `README.md` is a markdown file and serves as the entry point for the repository. It should be provide concise details on how to install and run the code along with the scope it covers. A good `README.md` could be the difference between someone using your code or not.

> GitHub and GitLab both offer the option to initialise a repository with a `README.md`.

--

> The API (Application Programming Interface) documention explains how to use the various components that make up your code (i.e. functions, classes, or other objects). This is extremely useful for other developers, but also for users if your code acts as a library (like e.g. [Numpy](https://numpy.org/)).

--

<img src="https://www.jetbrains.com/pycharm/guide/assets/sphinxdoc-5743f4ae.png" alt="Sphinx logo" width="200" class="reveal.imgblock">

> [Sphinx](https://www.sphinx-doc.org/) is a package that automatically generates API documentation from *docstrings* in Python code.

--

> We can start building our API documentation by running the following.

```bash
mkdir docs
sphinx-quickstart docs
```

> Note that this only needs to be done once.
<!-- .element: style="font-size: 50%;" -->

> This will add some content to the `docs` directory. 

--

> Inside `docs/source` we can find a file called `conf.py`, where the configuration options for Sphinx can be set. In particular, various Sphinx extensions. My personal preference is to set the following:

```python
extensions = [
    "sphinx.ext.autodoc",
    "sphinx.ext.autosummary",
    "sphinx.ext.doctest",
    "sphinx.ext.intersphinx",
    "sphinx.ext.todo",
    "sphinx.ext.coverage",
    "sphinx.ext.mathjax",
    "sphinx.ext.ifconfig",
    "sphinx.ext.viewcode",
    "sphinx.ext.napoleon",
    "sphinx.ext.intersphinx",
    "sphinx.ext.autosectionlabel",
    "numpydoc",
]
```

--

> We can now build the source `.rst` files for the modules in our Python package

```bash
sphinx-apidoc --module-first -feo docs/source mycosmo
```

> and finally the HTML files we want to navigate.

```bash
sphinx-build docs/source docs/build
```

> If we open `docs/build/index.html` we can see the results. Not very useful so far 😑.

--

> We need to add some docstrings to our modules and functions. Open `cosmology.py` and add the following to the very top of the file.

```python
"""Cosmology.

This module implements various cosmology routines.

"""
```

> Now rebuild the HTML files.

> You should see the information we added for the corresponding module.

--

> Now, let's add a more detailed docstring to our `hubble` function right after `def hubble(redshift, cosmo_dict):`.

```python
    r"""Hubble Parameter.

    Calculate the Hubble parameter at a given redshift.

    Parameters
    ----------
    redshift : float
        Redshift
    cosmo_dict : dict
        Dictionary of cosmological constants

    Returns
    -------
    float
        Value of the Hubble parameter

    Notes
    -----
    Implements

    .. math::
        H(z) = \sqrt{H_0^2 \Omega_{m,0}(1+z)^3 + \Omega_{Lambda,0} +
            \Omega_{K,0}(1+z)^2}

    """
```

--

> If you rebuild the docs again, you should see a lot more information along with some rendered LaTeX!

> Hoever, The default look is still quite... 🤮

> We can fix this by choosing a new [theme](https://sphinx-themes.org/) in `conf.py`

```python
html_theme = "sphinx_book_theme"
```

> and rebuilding the HTML. Much better! 🤩

--

## Exercise

> Add docstrings to the `critical_density` function and the remaining modules (`constants.py`, `__init__.py`). Rebuild the HTML to make sure everything renders correctly and make sure all the `PYDOCSTYLE` tests are passing when you run `pytest`.

> As with the [previous exercise](#/5/7), use the [Git workflow](#/4/18) you learned to implement the changes via a MR/PR.