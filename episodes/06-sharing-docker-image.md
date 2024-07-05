---
title: "Sharing your docker image"
teaching: 10
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- How to share a container image in Docker Hub?
- How to build and share a container image through GitHub

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Learn how to push a container image to Docker Hub
- Learn how to trigger the container image build through GitHub actions and share it through a GitHub Package.

::::::::::::::::::::::::::::::::::::::::::::::::

## Overview

In the previous episode you learnt how to build a container image.

In this episode, we will go through two alternatives for sharing the image.

## Share an image through Docker Hub

Create an account in the Docker image registry: go to [Docker Hub](https://hub.docker.com/) and click on "Sign up". This is a separate account from GitHub or anything else.

On the terminal, log in to Docker Hub:

```bash
docker login --username=<yourdockerhubname>
```

Tag the image with your username:

```bash
docker tag ml-python:v0.1 <yourdockerhubname>/ml-python:v0.1
```

::::::::::::::::::::::::::::::::: warning

Note that the ML image is big, the ML packages made it double the size of the base image!

If you want a quick exercise, use your emoji image instead:

```bash
docker tag python-emoji:v0.1 <yourdockerhubname>/python-emoji:v0.1
```

:::::::::::::::::::::::::::::::::::::::::

Push the image to Docker Hub:

```bash
docker push <yourdockerhubname>/ml-python:v0.1
```

or, for a quick check 

```bash
docker push <yourdockerhubname>/python-emoji:v0.1
```

Once done, if your Docker Hub repository is public, anyone see your image in `https://hub.docker.com/u/<yourdockerhubname>` and can pull it from it.


::::::::::::::::::::::::::::::::::::: challenge

### Document your image

Add a description and an overview.

:::::::::::::::: solution

Find your image in `https://hub.docker.com/u/<yourdockerhubname>`.
Add a brief descripion. In "Overview", write usage instructions.

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Exchange images

Try someone else's image.

Are the instructions clear?

:::::::::::::::::::::::::::::::::::::::::::::::

## Build and share an image through GitHub

You can also use GitHub actions to build the image and share it through GitHub packages. 

Your repository should contain a `Dockerfile` and all the files needed in it.

Create a `.github/workflows/` directory (with that exact name) in your repository:

```bash
mkdir .github
mkdir .github/workflows
```

and save a file `docker-build-deploy.yaml` in it with the following content:

```
name: Create and publish a Docker image

on:
  push:
    branches:
      - main

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build-and-push-image:
    runs-on: ubuntu-latest

    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Log in to the Container registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Docker Metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

Before pushing to the GitHub repository, exclude files you do not want to push in a `.gitignore` file.

Then check the status to make sure that you commit and push what you want

```bash
git status
```

The add and commit

```bash
git add .
git commit -m "Adding a worflow to build the container"
```

Check the remote to see that the push will go where you want

```bash
git remote -v
```

and push the changes

```bash
git push origin main
```

Once done, click on "Actions" in the GitHub Web UI and you will see the build ongoing:

![](fig/Screenshot_github_workflow_action.png)

If all goes well, after some minutes, you will see a green tick mark and the container image will be available in Packages

`https://github.com/<yourgithubname>/<repository>/pkgs/container/<repository>`

The image name is the same as the repository name because we have chosen so on line 10 of the `docker-build-deploy.yaml` file.

## Learn more

Learn more about Dockerfile instructions in the [Dockefile reference](https://docs.docker.com/reference/dockerfile/).

Learn more about writing Dockerfiles in the [HSF Docker tutorial](https://hsf-training.github.io/hsf-training-docker/06-dockerfiles/index.html).

Learn more about using Docker containers in the analysis work in the [Analysis pipelines training](https://indico.cern.ch/event/1375507/timetable/).


::::::::::::::::::::::::::::::::::::: keypoints 

- It is easy to build a new container image on top of an existing image.
- You can install packages that you need if they are not present in an existing image.
- You can add code and eventually compile it in the image so that it is ready to use when the container starts.

::::::::::::::::::::::::::::::::::::::::::::::::
