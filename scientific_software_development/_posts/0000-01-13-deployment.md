# Deployment

--

> With our packaging setup complete, we should ensure that everything on GitHub/GitLab is up to date with our local repo and working as expected. 🫡

> Once we are happy, we should *tag* the latest state of the `main` branch and create a *release* ([on GitHub](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository), [on GitLab](https://docs.gitlab.com/ee/user/project/releases/)). This way we can be sure that what we distribute is consistent with what we have tagged (i.e. a given version will have a consistent meaning both on the repo and on PyPI).

--

> Following the [Git workflow](#/5) we established previously, let's start by creating a feature branch for this work.

```bash
git checkout -b chore/add-license
```

--

> Before we bundle anything, it's worth adding a *licence* to our repository — this tells others what they are (and aren't) allowed to do with our code. Without one, the default under copyright law is that no one may reuse it at all.

> We will use the [MIT License](https://opensource.org/license/mit), a short, permissive licence that's a common default for small open-source projects.[$^{52}$](#/15/53)

```bash
touch LICENSE
```

--

```
MIT License

Copyright (c) 2026 <Author Name>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
<!-- .element: style="font-size: 40%;" -->

--

> Let's also record this in `pyproject.toml`, so it shows up in our package metadata on PyPI.

```toml
license = "MIT"
```

```bash
git add LICENSE pyproject.toml
git commit -m "Add MIT licence"
git push origin chore/add-license
```

> Then open a Pull/Merge request as explained in the [previous section](#/5) before merging the changes.

--

> Now, we are all set to bundle our package! `uv` can build it directly.[$^{53}$](#/15/54)

```bash
uv build
```

> This will create a `dist` (distribution) directory. Inside we will find a *wheel* (`.whl`) and compressed *tarball* (`.tar.gz`) of the package. These are the files that need to be uploaded to PyPI.

> `uv` can also handle the upload itself.

--

> First, we can do a dry run to make sure the distribution is OK, without actually uploading anything.

```bash
uv publish --dry-run
```

> If everything is good, it should complete without any errors.

--

<img src="https://upload.wikimedia.org/wikipedia/commons/6/64/PyPI_logo.svg" alt="PyPI logo" width="200" class="reveal.imgblock">

> Next, we will need to create an account on [PyPI](https://pypi.org/) (if we have not alredy done so).

--

> Before actually uploading to the official PyPI registry, we may want to make sure the package looks OK on the [Test PyPI](https://test.pypi.org/).[$^{54}$](#/15/55)

```bash
uv publish --publish-url https://test.pypi.org/legacy/
```

--

> Once we are happy with everything, we can upload to the offical PyPI registry.[$^{55}$](#/15/56)

```bash
uv publish
```

--

> Now the world can download our package!

```bash
pip install mycosmo
```

#### 🥳
