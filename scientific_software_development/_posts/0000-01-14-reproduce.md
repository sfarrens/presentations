# Reproducible research

--

> A final point to take into consideration is ensuring some backwards compatiblilty of our code for the sake of *reproducible research*. The lack of reproducibility of some key scientific results in recent years has become a serious problem. Enmourmous data sets and extensive pipelines with changing parts mean that it can be almost impossible to recreate a given result exactly.

> That doesn't mean, however, that we can't make an effort to guarantee as much reproducibility as possible. 🫡

--

> We actually already have most of what we need for this: `uv.lock`. Since it pins the exact version of every dependency, direct and transitive, anyone who runs `uv sync` against our repository gets byte-for-byte the same environment we have — no separate reproducibility tooling required.[$^{56}$](#/15/57)

> Some other useful tools for defining consistent environments more broadly are [Conda](https://docs.conda.io/) and [Docker](https://www.docker.com/).

--

> Following the [Git workflow](#/5) we established previously, let's start by creating a feature branch for this work.

```bash
git checkout -b chore/add-reproducibility-config
```

--

<img src="https://upload.wikimedia.org/wikipedia/commons/e/ea/Conda_logo.svg" alt="Conda logo" width="200" class="reveal.imgblock">

> [Conda](https://docs.conda.io/) is an open-source environment and package manager that allows the user to install pre-built binaries of specific packages. It remains popular for projects that need non-Python binary dependencies — `mycosmo` doesn't, since `uv.lock` already covers us, but it's worth knowing.

--

> We can provide a Conda `environment.yml` file with our package that will specify the specific version of Python we require as well along with all of the dependencies and their corresponding versions (if needed).

```bash
touch environment.yml
```

--

> Inside this file we could add the following content for `mycosmo`.

```yml
name: mycosmo
channels:
  - conda-forge
dependencies:
  - python=3.12
  - numpy>=1.25
```

--

> Let's add and commit this file.

```bash
git add environment.yml
git commit -m "Add Conda environment file"
```

--

> To build this environment the user would run the `create` command.

```bash
conda env create -f environment.yml
```

> Then to activate it, the user would run the `activate` command.

```bash
conda activate mycosmo
```

> This would provide the user with an environment with everything needed to run our code. 

--

> We can use Conda to create an environment that specifies exactly which versions of the packages are working for that release.[$^{57}$](#/15/58)

```yml
name: mycosmo
channels:
  - conda-forge
dependencies:
  - python=3.12
  - numpy==1.25
```

--

> This way we can (hopefully) avoid a situation where something changes in one of the packages that we use and breaks our code or indeed changes the results. 😱

--

<img src="https://upload.wikimedia.org/wikipedia/commons/4/4e/Docker_%28container_engine%29_logo.svg" alt="Docker logo" width="200" class="reveal.imgblock">

> [Docker](https://www.docker.com/) is a platform that provides *OS-level virtualisation* via a container system. This allows users to define a dedicated virtual operating system with all the required dependencies pre-installed.

--

> We can provide a `Dockerfile` with our package that will define a whole virtual operating system with everything set in a way that we know works with our software. 

```bash
touch Dockerfile
```

--

> Inside this file we could add the following content for `mycosmo`, using the same `uv` base image we already used for GitLab CI.[$^{58}$](#/15/59)[$^{59}$](#/15/60)

```dockerfile
FROM ghcr.io/astral-sh/uv:python3.12-bookworm-slim

LABEL Description="MyCosmo Docker Image"
WORKDIR /home

COPY . .

RUN uv sync --frozen
```

--

> Let's add and commit this file too.

```bash
git add Dockerfile
git commit -m "Add Dockerfile"
```

--

> To build the corresponding image, we (or the user), would simply use the `build` command.

```bash
docker build -t mycosmo .
```

> Then to launch an interactive container the user would use the `run` command.[$^{60}$](#/15/61)

```bash
docker run -it mycosmo
```

> Once inside, `uv run pytest`, `uv run python`, etc. all work exactly as they do locally.

--

> This would provide the user with a stable container in which the code should work exactly as expected. If Docker images are build for each release of the code, then it should be possible to reproduce the results that were produced at any given time.

> There are some things we simply cannot control, however, such as the physical architecture on which the code is run — Docker containers are fairly universal, but there's no 100% guarantee. This isn't perfect, but it goes a long way towards maintaining good standards of reproducible research.

--

> Let's push these changes and open a Pull/Merge request.

```bash
git push origin chore/add-reproducibility-config
```

> Then open a Pull/Merge request as explained in the [previous section](#/5) before merging the changes.
