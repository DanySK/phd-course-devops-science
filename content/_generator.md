+++

title = "Development + Operations"
description = "No silos"
outputs = ["Reveal"]

[reveal_hugo.custom_theme_options]
targetPath = "css/custom-theme.css"
enableSourceMap = true

+++

# DevOps for scientific research

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

## Building and sharing images

`docker build`
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
docker push

---

# Archival copies

---

# Licenses

---

# GitHub Pages
