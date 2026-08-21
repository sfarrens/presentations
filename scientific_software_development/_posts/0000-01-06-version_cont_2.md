## Version Control and Packaging III
### Git Repositories

--

> Now that we are all familiar with the basics of *Git* (see [previous section](#/3)), we can look at some of the cloud-based platforms that allow us to host Git repositories. In particular, we will look at [GitHub](https://github.com/) and [GitLab](https://about.gitlab.com/) and some of the tools they offer.

> We will also look at the `git` commands that allow us to interface with these platforms.

--

> Both GitHub and GitLab have their own strengths and weaknesses. For your own projects you should choose whichever platform you prefer. It is useful, however, to be familiar with both as for some projects you will have to go along with the platform chosen by the team.[$^{11}$](#/17/12)

--

|                     | GitHub | GitLab |
| ------------------- | ------ | ------ |
| Free                | ✅      | ✅      |
| Forking             | ✅      | ✅      |
| Mirroring           | ✅      | ⭐️      |
| Pages               | ✅      | ✅      |
| CI/CD               | ⭐️      | ✅      |
| Wiki                | ✅      | ⭐️      |
| Discussions         | ✅      | ❌      |
| Issue boards        | ✅      | ⭐️      |
| Pull/Merge requests | ✅      | ✅      |
| Containers          | ✅      | ✅      |
<!-- .element: style="font-size: 80%;" -->

--

> GitHub also recognises several special files that round out what it calls a repository's *Community Standards* — worth adding once a repo has some real activity.

--

- `CONTRIBUTING.md`: how others should propose changes ([docs](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions))
- `CODE_OF_CONDUCT.md`: expected behaviour for contributors ([docs](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-a-code-of-conduct-to-your-project))
- `SECURITY.md`: how to report vulnerabilities privately ([docs](https://docs.github.com/en/code-security/getting-started/adding-a-security-policy-to-your-repository))
- `.github/ISSUE_TEMPLATE/` and `.github/PULL_REQUEST_TEMPLATE.md`: structured templates for issues and PRs ([docs](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests))
- `CITATION.cff`: machine-readable citation metadata — GitHub shows a "Cite this repository" button when it's present ([spec](https://citation-file-format.github.io/))

--


> Let's go ahead and create a repository called `mycosmo` on either of the two platforms.*

> *Or both if you want.
<!-- .element: style="font-size: 50%;" -->

--

<img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" alt="GitHub logo" width="200" class="reveal.imgblock">

## GitHub

<mermaid>
graph LR;
    a["Repositories"]
    b["New"]
    c["Create repository"]
    a --> b --> |"Repo name: `mycosmo` 
    Leave the rest alone"| c
</mermaid>
<!-- .element: style="height: 150px;" -->

--

<img src="https://upload.wikimedia.org/wikipedia/commons/e/e1/GitLab_logo.svg" alt="GitLab logo" width="200" class="reveal.imgblock">

## GitLab

<mermaid>
graph LR;
    a["Projects"]
    b["New project"]
    c["Create blank project"]
    d["Create project"]
    a --> b --> c -->|"Project name: `mycosmo` 
    Unclick the `README` option
    Leave the rest alone"| d
</mermaid>
<!-- .element: style="height: 150px;" -->

--

> In order to connect our local (i.e. on your computer) Git repository with a hosting platform, we will need to provide an address for our *remote* (i.e. online) repository. We can do this with the `remote` command.

```bash
git remote add origin <REPOSITORY ADDRESS>
```

> Where, `origin` is just an alias for the remote address.[$^{12}$](#/17/13)

--

> We can list the currently attached remote addresses with the `-v` option.

```bash
git remote -v
```

--

> We can use the `push` command to upload our local repository to the remote hosting platform.[$^{13}$](#/17/14)

```bash
git push -u origin main
```

> Now, we can see our code and the corresponding commit history on either GitHub or GitLab.

--

> Both platforms support *issues*, and these are a great way of managing a software development project. 

> Let's start by creating an issue to refactor part of our code. With our last commit we introduced some hard-coded variables into a function. Let's move these into a separate module to make things easier to maintain.

> Then we can add our issue to a *project/issue board* and/or a *milestone* to keep track of its progress.

--

> Let's now look at some more `git` commands that will allow us manage a *Git workflow*.

> We can start by trying to address the issue we just opened. First, we need to create a new feature branch, let's call it `refactor/extract-constants`.

```bash
git checkout -b refactor/extract-constants
```

--

> To refactor our code, we will add a new file to `src/mycosmo` called `constants.py`.

```bash
touch src/mycosmo/constants.py
```

> and paste in the following content.

```python
G = 6.6743e-11
Mpc = 3.08568e22
```

--

> Now, we need to add an import of these constants to `cosmology.py` as follows

```python
import numpy as np

from .constants import Mpc, G
```

--

> and to modify the `critical_density` function as follows.

```python
def critical_density(redshift, cosmo_dict):
    H_z_si = hubble(redshift, cosmo_dict) * 1e3 / Mpc

    return (3.0 * H_z_si**2) / (8.0 * np.pi * G)
```

> Now, we will add and commit our changes.[$^{14}$](#/17/15)

```bash
git add -A
git commit -m "Extract constants into separate module"
```

--

> Instead of merging these changes locally, we will push the feature branch to our remote repository.

```bash
git push origin refactor/extract-constants
```

> Then we can open a *Pull Request* (PR, GitHub) or a *Merge Request* (MR, GitLab) to propose a solution to our issue. This will allow us to review the code, propose improvements and discuss the changes before merging to the `main` branch. 

--

> When working on a collaborative project, it is good practice to assign a *developer* (i.e. the person making the changes to the code) and a separate *reviewer* (i.e. the person who will check the merge/pull request) for each issue. When both parties are in agreement, the MR/PR can be merged.


#### 👍

--

Now, we have to clean everything up! 

#### 😅

--

> Start by deleting the remote feature branch (i.e. `refactor/extract-constants` on GitHub/GitLab).[$^{15}$](#/17/16)[$^{16}$](#/17/17)

> Then you need use the `pull` command to download the changes to the `main` branch.

```bash
git checkout main
git pull origin main
```

> Now, we can delete the local feature branch.[$^{17}$](#/17/18)

```bash
git branch -d refactor/extract-constants
```

--

We can complete the process we started by closing our issue!


#### 🥳

--

> Now, you are all set to continue your Git workflow.

<mermaid>
flowchart RL
    main_l["main"]
    main_r["main"]
    feat_l["feature"]
    feat_r["feature"]
    feat_l-->|`git push origin ...`|feat_r
    main_r-->|`git pull origin main`|main_l
    subgraph Remote
    feat_r-->|Pull/Merge Request|main_r
    end
    subgraph Local
    main_l-->|`git checkout -b ...`|feat_l
    end
</mermaid>
<!-- .element: style="height: 500px;" -->