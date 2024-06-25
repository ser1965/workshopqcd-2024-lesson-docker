---
title: "Docker containers for Monte Carlo generators and Rivet"
teaching: 15
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- What container images are available for MC generators ?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Download a container image with Pythia and Rivet
- Learn how to use the container

::::::::::::::::::::::::::::::::::::::::::::::::

## Overview

You will learn about [Rivet](https://gitlab.com/hepcedar/rivet) during this QCD school. The Rivet project provides [container images](https://gitlab.com/hepcedar/rivet/-/blob/release-3-1-x/doc/tutorials/docker.md) with Rivet and Monte Carlo generators included. For now, we will download the Rivet container images with Pythia and Sherpa code included.


## Rivet containers

The first time you start a container, a docker image file gets downloaded from an image registry. The Rivet image sizes are of order of 3 GB and it may take some time to download, depending on the speed of your internet connection. After the download, a container created from that image starts. The image download needs to be done only once. Afterwards, when starting a container, it will find the downloaded image on your computer, and it will be much faster.

The containers do not have modern editors and it is expected that you mount your working directory from the local computer into the container, and use your normal editor for editing the files. Note that all your compiling and executing still has to be done in the Docker container!

First, before you start up your container, create a local directory where you will be doing your code development. In the examples below, it is called `pythia_work` or `sherpa_work`. You may choose a different directory name if you like.

## Start the Pythia container

Create the shared directory in your working area:

```bash
mkdir pythia_work
cd pythia_work
```

Start the container with

```bash
docker run -v $PWD:/host -it --rm hepstore/rivet-pythia
```

This will share you current working directory (`$PWD`) into the container's `/host` directory.

The flag `--rm` makes docker delete the local container when you exit and every time you run this command it creates a new container. This means that changes done anywhere else than in the shared `/host` directory will be deleted.

Note that the `--rm` flag does not delete the image that you just downloaded.


You will get a container prompt similar this:

```output
root@3bbca327eda3:/work#
```

This is a bash shell in the container. Change to the shared `/host` directory.

```bash
cd /host
```

You will be doing all your work in this shared directory.
You can also set this directory as the working directory with flag `-w /host` in the `docker run...` command.

For now, exit from the container:

```bash
exit
```

List your local Docker images and containers and see that you have the `hepstore/rivet-pythia` image but no local containers as they were deleted with the `--rm` flag.

```bash
docker image ls
docker container ls -a
```

## Start the Sherpa container

Move back from the pythia working area with

```bash
cd ..
```

Create the shared directory in your working area:

```bash
mkdir sherpa_work
cd sherpa_work
```

Start the container with

```bash
docker run -v $PWD:/host -w /host -it --rm hepstore/rivet-sherpa:3.1.7-2.2.12
```

This will share you current working directory (`$PWD`) into the container's `/host` directory.

The flag `--rm` makes docker delete the local container when you exit and every time you run this command it creates a new container. This means that changes done anywhere else than in the shared `/host` directory will be deleted.

Note that the `--rm` flag does not delete the image that you just downloaded.


You will get a container prompt similar this:

```output
root@2560cb95ef24:/host#
```

The flag `-w /host` has set `/host` as the working directory. You will be doing all your work in this shared directory.

For now, exit from the container:

```bash
exit
```

## Test the environment

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1

Create a file on your local host and make sure that you can see it in the container. 

:::::::::::::::: solution

Open the editor in your host system, create a file `example.txt` and save it to the shared working area in `pythia_work`.

Start a container in your shared `pythia_work` directory:

```bash
docker run -v $PWD:/host -w /host -it --rm hepstore/rivet-pythia
```

In the container prompt, list the files and show the contents of the newly created file:

```bash
ls
cat example.txt
```

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 2

Create a file in the container and make sure that you can see it on your local area.

:::::::::::::::: solution

If you are not already in the container, start it with

```bash
docker run -v $PWD:/host  -w /host -it --rm hepstore/rivet-pythia
```

Write some output to a file

```bash
echo "This is from the container" > output.txt
```

Exit from the container and see that you have the output file on your local area.

```bash
exit
ls
cat output.txt
```

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 3

Check the Rivet version in the Sherpa container. 

:::::::::::::::: solution

Start a container in your shared `sherpa_work` directory:

```bash
docker run -v $PWD:/host -w /host -it --rm hepstore/rivet-sherpa:3.1.7-2.2.12
```

In the container prompt, check the Rivet version with

```bash
rivet --version
```

You should get

```output
rivet v3.1.7
```

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 4

In the Pythia container, copy a Pythia example code `main01.cc`, and `Makefile` and `Makefile.inc` from `/usr/local/share/Pythia8/examples/` of the container to your shared area. Compile the code with `make` and run it.

:::::::::::::::: solution

Start the Pythia container in your shared `pythia_work` directory:

```bash
docker run -v $PWD:/host -w /host -it --rm hepstore/rivet-pythia
```

Copy the files to your working area with

```bash
cp /usr/local/share/Pythia8/examples/main01.cc .
cp /usr/local/share/Pythia8/examples/Makefile* .
```

Compile and run

```bash
make main01
./main01
```

You will get a particle listing and a lovely ASCII histogram of charged multiplicity.

:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 

- You have now set up a docker container as a working enviroment Rivet and Pythia.
- You know how to share a working area between your own computer and the container.

::::::::::::::::::::::::::::::::::::::::::::::::
