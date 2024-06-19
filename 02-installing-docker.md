---
title: "Installing docker"
teaching: 15
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do you install Docker?
- What are the main Docker concepts and commands I need to know?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Install Docker
- Test the installation
- Learn and exercise the basic commands.

::::::::::::::::::::::::::::::::::::::::::::::::

## Installing docker

Go to the offical [Docker site and their installation instructions](https://docs.docker.com/get-docker/)
to install Docker for your operating system.

For our purposes, we need [docker-engine](https://docs.docker.com/engine/install/) which is a free open-source software. Note that you can choose to install Docker Desktop (free for single, non-commercial use), but in our instructions, we do not rely on the Graphical UI that it offers.

::: tab

### Windows WSL

Install [Docker Destop](https://docs.docker.com/get-docker/) following the WSL2 instructions or [docker-engine](https://docs.docker.com/engine/install/) following the Linux instructions (Ubuntu/debian).

For the docker-engine to work, if installed without Docker Desktop, [activate `systemd`](https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl/) on your WSL2.

### Mac

Install [Docker Destop](https://docs.docker.com/get-docker/)

### Linux

Install [Docker Destop](https://docs.docker.com/get-docker/) or [docker-engine](https://docs.docker.com/engine/install/) following the instruction for your Linux flavour.

:::



::::::::::::::::::::::::::::::::::::: callout

## Windows users:

In the episodes of this lesson that follow, we assume that Windows users have [WSL2](https://docs.microsoft.com/en-us/windows/wsl/install-win10) activated with a Linux bash shell (e.g. Ubuntu). All commands indicated with "bash" are expected to be typed in this Linux shell (not in git bash or power shell).

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::




## Testing

As you walk through their documentation, you will eventually come to a point where you will
run a very simple test, usually involving their `hello-world` container.

You can find their documentation for this step [here](https://docs.docker.com/get-started/).

Testing their code can be summed up by the ability to run (without generating any errors) the following
commands.

```bash
docker --version
```

```bash
docker run hello-world
```

::::::::::::::::::::::::::::::::::::: callout

## Important: Docker postinstall to avoid sudo for Linux installation

If you need `sudo` to run the command above, make sure to complete [these steps](https://docs.docker.com/engine/install/linux-postinstall/) after installations, otherwise you will run into trouble with shared file access later on. Guaranteed!!

In brief:

```bash
sudo groupadd docker #most likely exists already
sudo usermod -aG docker $USER
```

Then close the shell and open a new one. Verify that you can run `docker run hello-world` without `sudo`.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## Images and Containers

As it was mentioned above, there is [ample documentation](https://docs.docker.com/) provided by Docker official sites.  However, there are a couple of concepts that are crucial for the sake of using the container technology with CMS open data: **container images** and **containers**.

One can think of the **container image** as the main ingredients for preparing a dish, and the final dish as the **container** itself.  You can prepare many dishes (**containers**) based on the same ingredients (**container image**). Images can exist without containers, whereas a container needs to run an image to exist. Therefore, containers are dependent on images and use them to construct a run-time environment and run an application.

The final dish, for us, is a container that can be thought of as an isolated machine (running on the host machine) with mostly its own operating system and the adequate software and run-time environment to process CMS open data.

Docker provides the ability to create, build and/or modify images, which can then be used to create containers.  We will not use this aspect of the technology because, as you will see later, we will use an already-built and ready-to-use image in order to create our needed container.

## Commands Cheatsheet

There are many [Docker commands](https://docs.docker.com/engine/reference/commandline/docker/) that can be executed for different tasks.  However, the most useful for our purposes are the following.  We will show some usage examples for some of these commands later.  Feel free to explore other commands.

### Download image:
```bash
docker pull <image>
```

### List images:
```bash
docker image ls
```

### Remove images
```bash
docker image rm <image>
```
or
```bash
docker rmi <image>
```

### List containers
```bash
docker container ls -a
```
  or
```bash
docker ps -a
```
The `-a` option shows all containers (default shows just those running)


### Remove containers
```bash
docker container rm <container>
```
or
```bash
docker rm <container>
```

### Create and start a container based on a specific image
```bash
docker run [options] <image>
```
This command will be used later to create our CMS open data container.

The option `-v` for mounting a directory from the local computer to the container will also be used so that you can edit files on your normal editor and used them in the container:
```bash
docker -v <directory-on-your-local-computer>:<directory-in-the-container> <image>
```

### Stop a running container
```bash
docker stop <container>
```

### Start a container that was stopped
```bash
docker start -i <container>
```

### Copy files in or out of a container
```bash
docker cp <container>:<path> <local path>
docker cp <local path> <container>:<path>
```


::::::::::::::::::::::::::::::::::::: keypoints 

- For up-to-date details for installing Docker, the official documentation is the best bet.
- Make sure you were able to download and run Docker's `hello-world` example.
- The concepts of image and container, plus the knowledge of certain Dockers commands, is all that is needed for the hands-on sessions.

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
