---
title: "WIP: Building your own docker image"
teaching: 10
exercises: 10
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

But first, let us see from where we got the images that we have been using so far.

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

We will use a small basic Python image as example and add one Python package in it.

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

### Test that Python package is there

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

Make a new subdirectory (e.g. `test`) and save the code in a file (e.g. `myscript.py`) there, with

```
from emoji import emojize as em
print(em(":ghost:"))
```

Now, you must pass this into the container using the `-v` flag in the `docker run ...` command. We know to use the container directory `/usr/src` from the Python container image [usage instructions](https://hub.docker.com/_/python).

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

In the test directory and save a file (e.g. `printme.py`) with

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

As the command has become long, you could define it as "alias". First, verify what aliases you have:

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

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

## Add code to the image

If the code that you use is stable, you can add it to the container image.

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

Start the container in its local bash shell:

```bash
docker run -it --rm python-emoji /bin/bash
```

Note that the container prompt indicates the working directory. List the contents:

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



::::::::::::::::::::::::::::::::::::: keypoints 

- It is easy to build a new container image on top of an existing image.
- You can install packages that you need if they are not present in an existing image.
- You add code and eventually compile it in the image so that it is ready to use when the container starts.

::::::::::::::::::::::::::::::::::::::::::::::::
