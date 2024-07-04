---
title: "Building your own docker image"
teaching: 10
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- What if the container image does not have what you need installed?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Learn how to build a container image from an existing image
- Use the container image locally

::::::::::::::::::::::::::::::::::::::::::::::::

## Overview

You might be using a container image, but it does not have everything you need installed. Therefore, every time you create a container from that image, you will need to install something in it. To save time, you can build an image of your own with everything you need.

Building an image from an existing image is easy and can be done with `docker` commands.

We will now exercise how to do it, first with a simple example and then with a container image that we have been using in the exercises.

But first, let us see where we get the images that we have been using so far.

## Container image registries

Container images are available from [container registries](https://docs.docker.com/guides/docker-concepts/the-basics/what-is-a-registry/). In the previous episodes, we have used the following images:

- `hello-world`
- `gitlab-registry.cern.ch/cms-cloud/python-vnc:python3.10.5`
- `gitlab-registry.cern.ch/cms-cloud/root-vnc:latest`
- `hepstore/rivet-pythia`
- `hepstore/rivet-sherpa:3.1.7-2.2.12`

The syntax for the image name is: `[registry_name]/[repository]/image_name:[version]`

The **registry name** is like a domain name. If no registry name is given, the default location is the docker image registry: [Docker Hub](https://hub.docker.com/), this is the case for `hello-world` and the MC generator images. CMS open data images come from the container image registry of a CERN GitLab project.

The **repository** is a collection of container images within the container registry. For the MC generator images, it is `hepstore` in Docker Hub, for the CMS open data images, it is `cms-cloud` in the CERN GitLab.

A **version** tag comes after `:`. If no value is given, the latest version is taken. Also, `:latest` takes the most recent tag. Note that versions may change, and it is good practice to specify the version.

## Building an image

We will use a small Python image as a base image and add one Python package in it.

Python container images are available from the Docker Hub: https://hub.docker.com/_/python

There's a long list of them, and we choose a tag with

- "slim" to keep it small and fast
- "bookworm" for a stable [Debian release](https://wiki.debian.org/DebianReleases).

The recipe for the image build is called `Dockerfile`. Create a new working directory, e.g. `docker-image-build` and move to it.

```bash
mkdir docker-image-build
cd docker-image-build
```

Choose an example Python package to install, for example [emoji](https://pypi.org/project/emoji/).

Create a new file named `Dockerfile` with the following content

```
FROM python:slim-bookworm
RUN pip install emoji
```

Build a new image with 

```bash
docker build --tag python-emoji .
```

The command `docker build` uses `Dockerfile` (in the directory in which it is run) to build an image with name defined with `--tag`. Note the dot (.) at the end of the line.

```output
[+] Building 37.2s (7/7) FINISHED                                                                        docker:default
 => [internal] load build definition from Dockerfile                                                               0.1s
 => => transferring dockerfile: 85B                                                                                0.0s
 => [internal] load .dockerignore                                                                                  0.1s
 => => transferring context: 2B                                                                                    0.0s
 => [internal] load metadata for docker.io/library/python:slim-bookworm                                            5.3s
 => [auth] library/python:pull token for registry-1.docker.io                                                      0.0s
 => [1/2] FROM docker.io/library/python:slim-bookworm@sha256:da2d7af143dab7cd5b0d5a5c9545fe14e67fc24c394fcf1cf15  23.1s
 => => resolve docker.io/library/python:slim-bookworm@sha256:da2d7af143dab7cd5b0d5a5c9545fe14e67fc24c394fcf1cf15e  0.0s
 => => sha256:da2d7af143dab7cd5b0d5a5c9545fe14e67fc24c394fcf1cf15e8ea16cbd8637 9.12kB / 9.12kB                     0.0s
 => => sha256:daab4fae04964d439c4eed1596713887bd38b89ffc6bdd858cc31433c4dd1236 1.94kB / 1.94kB                     0.0s
 => => sha256:d9711c6b91eb742061f4d217028c20285e060c3676269592f199b7d22004725b 6.67kB / 6.67kB                     0.0s
 => => sha256:2cc3ae149d28a36d28d4eefbae70aaa14a0c9eab588c3790f7979f310b893c44 29.15MB / 29.15MB                  15.7s
 => => sha256:07ccb93ecbac82807474a781c9f71fbdf579c08f3aca6e78284fb34b7740126d 3.32MB / 3.32MB                     3.4s
 => => sha256:aaa0341b7fe3108f9c2dd21daf26baff648cf3cef04c6b9ee7df7322f81e4e3a 12.01MB / 12.01MB                  10.0s
 => => sha256:c9d4ed0ae1ba79e0634af2cc17374f6acdd5c64d6a39beb26d4e6d1168097209 231B / 231B                         4.4s
 => => sha256:b562c417939b83cdaa7dee634195f4bdb8d25e7b53a17a5b8040b70b789eb961 2.83MB / 2.83MB                     8.6s
 => => extracting sha256:2cc3ae149d28a36d28d4eefbae70aaa14a0c9eab588c3790f7979f310b893c44                          4.3s
 => => extracting sha256:07ccb93ecbac82807474a781c9f71fbdf579c08f3aca6e78284fb34b7740126d                          0.3s
 => => extracting sha256:aaa0341b7fe3108f9c2dd21daf26baff648cf3cef04c6b9ee7df7322f81e4e3a                          1.3s
 => => extracting sha256:c9d4ed0ae1ba79e0634af2cc17374f6acdd5c64d6a39beb26d4e6d1168097209                          0.0s
 => => extracting sha256:b562c417939b83cdaa7dee634195f4bdb8d25e7b53a17a5b8040b70b789eb961                          0.8s
 => [2/2] RUN pip install emoji                                                                                    8.3s
 => exporting to image                                                                                             0.2s
 => => exporting layers                                                                                            0.1s
 => => writing image sha256:f784d45ea915a50d47b90a3e2cdd3a641c1fd75bc57725f2fb5325c82cf66d1b                       0.0s
 => => naming to docker.io/library/python-emoji                                                                    0.0s

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview
```

You can check with ` docker images` that the image has appeared in the list of images.

## Using the image locally


Start the container with

```bash
docker run -it --rm python-emoji
```

The flag `--rm` makes docker delete the local container when you exit.

It will give you a Python prompt. 

::::::::::::::::::::::::::::::::::::: challenge

### Test that the Python package is in the container

Read from the [emoji package documentation](https://pypi.org/project/emoji/) how to use it and test that it works as expected.

:::::::::::::::: solution


```output
Python 3.12.4 (main, Jun 27 2024, 00:07:37) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from emoji import emojize as em
>>> print(em(":snowflake:"))
❄️
```

Exit with `exit()` or Ctrl-d.

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::: challenge

### Inspect the container from the bash shell 

Start the container bash shell and check what Python packages are installed.

Hint: use `/bin/bash` in the `docker run...` command.

:::::::::::::::: solution

Start the container in its local bash shell:

```bash
docker run -it --rm python-emoji /bin/bash
```

Use `pip list` to see the Python packages:

```bash
root@aef232a1babe:/# pip list
```

```output
root@aef232a1babe:/# pip list
Package           Version
----------------- -------
emoji             2.12.1
pip               24.0
setuptools        70.1.1
typing_extensions 4.12.2
wheel             0.43.0

[notice] A new release of pip is available: 24.0 -> 24.1.1
[notice] To update, run: pip install --upgrade pip

```

It is often helpful to use bash shell to inspect the contents of a container.

Exit from the container with `exit`.

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Run a single Python script in the container

Sometimes, it is convenient to pass a single Python script to the container.
Write an example Python script to be passed into the container.

:::::::::::::::: solution

Make a new subdirectory (you can name it `test`) and save the Python code in a file (name it `myscript.py`):

```
from emoji import emojize as em
print(em(":ghost:"))
```

Now, you must pass this script into the container using the `-v` flag in the `docker run ...` command. We know to use the container directory `/usr/src` from the Python container image [usage instructions](https://hub.docker.com/_/python) and we choose to name the container directory `mycode`.

You can use the `-w` to open the container in that directory.

```bash
docker run -it --rm  -v "$PWD"/test:/usr/src/mycode -w /usr/src/mycode python-emoji python myscript.py
```

👻

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Make the Python script configurable

Make the Python script configurable so that you can use any of the [available emojis](https://carpedm20.github.io/emoji/). I'm sure you have tried a few already 😉

:::::::::::::::: solution

Save the following a file (e.g. `printme.py`) in the `test` directory:

```
from emoji import emojize as em
import sys

if __name__ == "__main__":
  anemoji = sys.argv[1]
  print(em(":"+anemoji+":"))
```

Run this with

```bash
docker run -it --rm  -v "$PWD"/test:/usr/src/mycode -w /usr/src/mycode python-emoji python printme.py sparkles
```

✨

:::::::::::::::: spoiler

### For fun 🎁

As the docker command has become long, you could define a short-cut, an "alias". First, verify your existing aliases:

```bash
alias
```

If `printme` is not in use already, define it as alias to the long command above:

```bash
alias printme="docker run -it --rm  -v "$PWD"/test:/usr/src/mycode -w /usr/src/mycode python-emoji python printme.py"
```

Now try:

```bash
printme party_popper
```

🎉

Have fun!


::::::::::::::::::::::::

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

## Add code to the image

If the code that you use in the container is stable, you can add it to the container image.

Create a `code` subdirectory and copy our production-quality `printme.py` script in it.

Modify the `Dockerfile` to use `/usr/src/code` as the working directory and copy the local `code` subdirectory into the container (the dot brings it to the working directory defined in the previous line):

```
FROM python:slim-bookworm
RUN pip install emoji

WORKDIR /usr/src/code

COPY code .
```

Build the image with this new definition. It is a good practice to use version tags:

```bash
docker build --tag python-emoji:v0.1 .
```

::::::::::::::::::::::::::::::::::::: challenge

### Inspect the new container 

Open a bash shell in the container and check if the file is present

:::::::::::::::: solution

Start a bash shell in the container:

```bash
docker run -it --rm python-emoji /bin/bash
```

Note that the container prompt indicates the working directory `/usr/src/code`. List the contents:

```bash
root@03efa1b45f1b:/usr/src/code# ls
```

```output
printme.py
```

Exit from the container with `exit`.

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Run the script 

Run the script that is now in the container.

:::::::::::::::: solution

Start the container 

```bash
 docker run -it --rm python-emoji python printme.py magic_wand
```

There's no need to pass the local directory into the container as the code is stored in the container image.

🪄


:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

## More complex Dockerfiles

Check if you can make sense of more complex Dockerfiles bases on what you learnt from our simple case.

::::::::::::::::::::::::::::::::::::: challenge

### Explore Dockerfiles 

Explore the various [hepforge](https://rivet.hepforge.org/) Dockerfiles. You can find them under the MC generator specific directories in the [Rivet source code repository](https://gitlab.com/hepcedar/rivet/-/tree/release-4-0-x/docker?ref_type=heads).

:::::::::::::::: solution

You can start from the [rivet-pythia image Dockerfile](https://gitlab.com/hepcedar/rivet/-/blob/release-4-0-x/docker/rivet-pythia/Dockerfile?ref_type=heads).

Find the familiar instructions `FROM`, `RUN`, `COPY`and `WORKDIR`.

Observe how the image is built `FROM` an existing image

```
FROM hepstore/rivet:${RIVET_VERSION}
```

Find the Dockerfile for that base image: [rivet Dockerfile](https://gitlab.com/hepcedar/rivet/-/blob/release-4-0-x/docker/rivet/Dockerfile?ref_type=heads).

See how it is built `FROM` another base image 

```
FROM hepstore/hepbase-${ARCH}-latex
```

and explore a [hepbase Dockerfile](https://gitlab.com/hepcedar/rivet/-/blob/release-4-0-x/docker/hepbase/Dockerfile.ubuntu?ref_type=heads).



:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

## Build a Machine Learning Python image

In the Machine Learning hands-on session, the first thing to do was to install some additional packages in the Python container. Can you build a container image with these packages included?

::::::::::::::::::::::::::::::::::::: challenge

### Build a new Python image with the ML packages 

Build a new Machine Learning Python image with the ML Python packages included. Use as the base image the new tag [python3.10.12](https://gitlab.cern.ch/cms-cloud/python-vnc/container_registry/14160) that includes a package needed for the ML exercises to work.

:::::::::::::::: solution

The container image `FROM` which you want to build the new image is `gitlab-registry.cern.ch/cms-cloud/python-vnc:python3.10.12` which has an update kindly provided for the ML exercise to work.

The Python packages were installed with

```bash
pip install qkeras==0.9.0 tensorflow==2.11.1 hls4ml h5py mplhep cernopendata-client pydot graphviz fsspec-xrootd
pip install --upgrade matplotlib
```

:::::::::::::::::: callout

Note that in the new image, you need to add `fsspec-xrootd` to the Python packages to install.

::::::::::::::::::::::::::

When you run the upgrade for `matplotlib`, you might have noticed the last line of the output `Successfully installed matplotlib-3.9.1`. So we pick up that version for `matplotlib`.

Write these packages in a new file called `requirements.txt`:

```
qkeras==0.9.0
tensorflow==2.11.1
hls4ml
h5py
mplhep
cernopendata-client 
pydot
graphviz
matplotlib==3.9.1
fsspec-xrootd==0.3.0
```

In the Dockerfile, instruct Docker to start `FROM` the base image, `COPY` the requirements file in the image and `RUN` the `pip install ...` command:

```
FROM gitlab-registry.cern.ch/cms-cloud/python-vnc:python3.10.12

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt
```

Build the image with

```bash
docker build --tag ml-python:v0.1 .
```

Start a new container from this new image and check with `pip list` if the packages are present. Since you do not need to install anything in the container, you can as well remove it after the start with the flag `--rm` (in the similar way as it was done with the MC generator containers):

```bash
docker run -P -p 8888:8888 -v $PWD:/code -it --rm ml-python:v0.1
```

```bash
cmsusr@c3ee9106792c:/code$ pip list
```

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### What can go wrong?

Can you spot a problem in the way the we built the image?
What will happen if you use the same Dockerfile in some months from now?

:::::::::::::::: solution

We defined some of the Python packages without a version tag. The current versions will get installed. That's fine if you intend to use them right now just once.

But check the release history of the packages at `https://pypi.org/project/<package>/#history`. You will notice that some of them get often updated.

While the container that you start from **this** image will be the same, it may well be that **next time** you build the image with this Dockerfile, you will not get the same packages.

It is always best to define the package versions. You can find the current versions from the listing of `pip list`. Update `requirements.txt` to

```
qkeras==0.9.0
tensorflow==2.11.1
hls4ml==0.8.1
h5py==3.11.0
mplhep==0.3.50
cernopendata-client==0.3.0
pydot==2.0.0
graphviz==0.20.3
matplotlib==3.9.1
fsspec-xrootd==0.3.0
```

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

## Learn more

Learn more about Dockerfile instructions in the [Dockefile reference](https://docs.docker.com/reference/dockerfile/).

Learn more about writing Dockerfiles in the [HSF Docker tutorial](https://hsf-training.github.io/hsf-training-docker/06-dockerfiles/index.html).

Learn more about using Docker containers in the analysis work in the [Analysis pipelines training](https://indico.cern.ch/event/1375507/timetable/).


::::::::::::::::::::::::::::::::::::: keypoints 

- It is easy to build a new container image on top of an existing image.
- You can install packages that you need if they are not present in an existing image.
- You can add code and eventually compile it in the image so that it is ready to use when the container starts.

::::::::::::::::::::::::::::::::::::::::::::::::
