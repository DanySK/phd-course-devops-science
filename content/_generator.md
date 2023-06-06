+++

title = "DevOps for scientific research"
description = "Reproducibility and replicability in scientific research, enforced."
outputs = ["Reveal"]

[reveal_hugo.custom_theme_options]
targetPath = "css/custom-theme.css"
enableSourceMap = true

+++

# DevOps meets scientific research

## *Danilo Pianini*

---

![image](https://user-images.githubusercontent.com/1991673/236500955-b65de211-3b51-4adf-b0e5-6f248771e479.png)

---

## Replication crisis
### aka replicability crisis or reproducibility crisis

* ongoing *methodological crisis*
* the results of many scientific studies are hard or impossible to *reproduce*.

* empirical reproductions are essential for the the scientific method

#### no reproducibility $\Rightarrow$ scientific credibility is undermined

---

## Countermeasures

### Specifically for data science and computer science

* Make your artifacts *available*
  * Share code as open source (**licensing**)
  * Share code and data where people will find it (**GitHub**)
  * Share code and data where it will be archived for the foreseeable future (**Zenodo**)
* Make your artifacts *reproducible*
  * It works on your PC? Ship your PC! (**containerization**)
* Make your artifacts *maintainable*
  * Be ready to accept contributions and work in team (**version control**)
  * Always check that the software is working (**continuous integration**)
* Make your artifacts *reusable*
  * document them appropriately (**GitHub Pages**)

Most techniques come from the DevOps world!

---

## Course contents

### Specifically for data science and computer science

* Version control with **git**
* Share code on **GitHub**
* Continuous integration in **GitHub Actions**
* Containerization via **Docker**
* Archive your artifacts on **Zenodo**
* Licensing
* Create documentation on **GitHub Pages**

---

## Course exam

**Create a reusable artifact**
* If you have a paper that is being submitted, create the artifact for it and it counts as a valid exam (if done properly)
* if you don't, then a toy project is fine as well

### Must-have
* Version control
* Appropriate License
* Continuous Integration
* Containerization
* Zenodo archiving
* GitHub Pages documentation

---

<!-- write-here "shared-slides/devops/devops-intro.md" -->

<!-- end-write -->

---

# Version control with git

---

<!-- write-here "shared-slides/git/dvcs-concepts.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/git/intro.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/git/basics-no-branching.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/git/branching-merging.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/git/merge-exercise.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/git/branch-deletion.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/git/remote-operations.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/git/tagging-basics.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/git/git-hosting-github.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/ci/intro.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/ci/core-concepts.md" -->

<!-- end-write -->

---

<!-- write-here "shared-slides/ci/gha.md" -->

<!-- end-write -->

---

# Containerization

---

## The problem: "it works on my machine"

![](https://i.imgflip.com/7nw0hk.jpg)

---

## The solution: ship your machine

Containers can be thought of as (but they *are not*) lightweight virtual machines:
* *Isolated* environments
* Contain the *whole runtime*
  * They use the host's kernel, but have their own system libraries
* *Ephemeral*
  * Storage and state are discarded on termination
* Generated from an **image**
  * We will *write images* and *launch containers*

### Containers for us

* Simple, reproducibile configuration
* R2-proof reproducibility (single command)
* Isolated test environments

---

## Docker

Docker is the most common containerization technology (standard de facto).

We will interact with Docker through its *Command Line Interface*

### Simplified Docker CLI

`docker [container|image] <command> <args>`

* `[container|image]` *optional* target
* `<command>` the *action* to perform
* `<args>` the command's arguments

---

## Managing *images*

`docker image`
* `ls`
  * List the locally available images
* `prune`
  * Remove unused images
* `pull`
  * Download an image from a registry (usually, dockerhub.io)
* `rm`
  * Remove one or more images

---

## Running *containers*

`docker [container] run <options> image`

Creates and executes a new container from `image`
* "`container`" can be omitted

### Options

* `-p <host>:<guest>` --- publishes (exposes) the container's port `<guest>` to host's port `<host>`
* `-v <host>:<guest>` --- *bind mount*: mounts absolute path `<host>` into the container at absolute path `<guest>`
* `-name <name>` --- assigns a unique name to the container

* `-i` --- interactive mode. Required to send commands to the container
* `-t` --- attaches a pseudo-TTY. Use the option to have the same feel of a terminal open in the container
run         Create and run a new container from an image
* `-e <key>=<value>` --- sets environment variable `<key>` to `<value>`
* `-d` --- detached: returns control to the host terminal and runs in background

---

## Managing *containers*

`docker container`
* `ls --all`
  * List the local containers (including the stopped ones)
* `exec <name> <command>`
  * Executes `<command>` in a running container named `<name>`
  * Supports many options of `run`: `-i`, `-t`, `-e`, `-d`
* `top`
  * Display the running processes of a container
* `rename <old> <new>`
  * Rename a container `<old>` to `<new>`
* `rm <names>`
  * Remove one or more containers
* `prune`
  * Remove all stopped containers

---

## Creating images

Images are defined in **Dockerfile**s
* Dockerfile $\Rightarrow$ Image $\Rightarrow$ Container
* Dockerfiles are a list of steps that modify an image to obtain a new one

```Dockerfile
# Starting point. SCRATCH to start from empty (not recommended)
FROM imagename:version

# Run a command in the current image. Side effects will be stored
RUN some command

# Copy a file into the image
COPY file/in/host /destination/in/image

# Set an environment variable
ENV MY_VARIABLE=MYVALUE

# Change directory
WORKDIR /my/new/directory

# Configure the container to run as an executable
ENTRYPOINT ["executable", "parameter", "parameter2"]

# Default command
CMD ["executable", "parameter", "parameter2"]
```

---

## Tagging images

Tagging is the operation of adding custom symbolic names (aliases) to images

`docker [image] tag <source_image> <target_image>`

Creates an alias for `<source_image>` named `<target_image>`

* "`image`" can be omitted

Tags are usually structured as `name:version`
  * `name` is typically in the form `owner_name/image_name`
    * *official* images have no `owner_name/`
  * `version` can be any string, but:
    * `latest` is a special version that identifies the most recent image
    * versions are normally assigned as numbers with dot separators (e.g., `3.10`)
      * Typically these numbers match those of the software inside the container
        * e.g., image `python:3.10` contains the Python interpreter at version `3.10`
    * additional information is stored in a dash-prefixed suffix
      * e.g., image `python:3.10-buster` contains the Python interpreter at version `3.10` and the runtime of *Debian Buster*

---

## Building images

`docker [image] build <options> <directory>`

Creates a new `image` from a directory containing a `Dockerfile`
* "`image`" can be omitted
* if launched from where the `Dockerfile` located:
  * `docker build .`

### Options

* `-t <tag>` --- adds tag `<tag>` to the image. Multiple tags can be specified.

#### Typical build command:
* `docker build -t my_name/my_project:latest -t my_name/my_project:1.0.0 .`

---

## Sharing images: selecting a registry and logging in

Images are fetched and stored in **registries**
* The most common registry for docker is `dockerhub.io`
* GitHub also has an [image registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry), `ghcr.io`

### Logging into a registry

`docker login <registry>`
* If `<registry>` is omitted, `dockerhub.io` is used
* Interactive login: `docker login <registry>`
* Non-interactive login
  * `docker login -u <username> -p <password> <registry>`
  * (better) `cat <password> | docker login -u <username> --password-stdin <registry>`

---

## Sharing images: pushing images

### Pushing images

`docker push <image>`
* Pushes `<image>` to the registry the user is currently logged into
* If your username is `<user>`, then the image must be tagged as `<user>/<name>:<version>`

Your image can now be pulled by anyone!

---

# Archival copies

---

## Open Science

* Open Science is a movement that aims to make scientific research,
data, and dissemination accessible to all levels of society
* A relevant part of Open Science is *accessibility* and *reproducibility*
* Public services exist that take snapshots of scientific artifacts and store them for the future, e.g.:
  * [Zenodo](https://zenodo.org/)
  * [Software Heritage](https://www.softwareheritage.org/)

Artefacts that are not archived are at a **high risk of being lost forever**!

With them, away goes the possibility of reproducing the results.

---

## Zenodo

Zenodo is a service that allows to archive and share scientific artifacts.
Its key features are:
* **Permanent** storage
* **DOI** assignment
* **GitHub integration**
* Managed by **CERN**
* No cost

### Archiving a repository on Zenodo

To automatically archive a repository on Zenodo:
1. *Connect* your GitHub account to Zenodo
2. In the project list of Zenodo, *enable* the repository to archive
3. Done! Every new *release* on GitHub will be archived on Zenodo
  * So, of course, make the releases on GitHub *automatic*!

---

# Licenses

---

<!-- write-here "shared-slides/licenses.md" -->

<!-- end-write -->

---

# GitHub Pages

---

<!-- write-here "shared-slides/ci/ghpages.md" -->

<!-- end-write -->
