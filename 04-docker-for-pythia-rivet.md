---
title: "Docker containers for Pythia and Rivet"
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

You will learn about [Rivet](https://gitlab.com/hepcedar/rivet) during this QCD school. The Rivet project provides [container images](https://gitlab.com/hepcedar/rivet/-/blob/release-3-1-x/doc/tutorials/docker.md) with Rivet and Monte Carlo generators included. For now, we will download the container image with Rivet and Pythia.


## Rivet containers

The first time you start a container, a docker image file gets downloaded from an image registry. The Rivet image sizes are of order of 3 GB and it may take some time to download, depending on the speed of your internet connection. After the download, a container created from that image starts. The image download needs to be done only once. Afterwards, when starting a container, it will find the downloaded image on your computer, and it will be much faster.

The containers do not have modern editors and it is expected that you mount your working directory from the local computer into the container, and use your normal editor for editing the files. Note that all your compiling and executing still has to be done in the Docker container!

First, before you start up your container, create a local directory where you will be doing your code development. In the examples below, it is called `rivet_work`. You may choose a different directory name if you like.

## Start the container

Create the shared directory in your working area:

```bash
mkdir rivet_work
cd rivet_work
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
For now, exit from the container:

```bash
exit
```

List your local Docker images and containers and see that you have the `hepstore/rivet-pythia` image but no local containers as they were deleted with the `--rm` flag.

```bash
docker image ls
docker ps -a
```


## Test the environment

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1

Create a file on your local host and make sure that you can see it in the container. 

:::::::::::::::: solution

Open the editor in your host system, create a file `example.txt` and save it to the shared working area in `rivet_work`.

Start a container in your shared `rivet_work` directory:

```bash
docker run -v $PWD:/host -it --rm hepstore/rivet-pythia
```

In the container prompt, move to `/host`, list the files and show the contents of the newly created file:

```bash
cd /host
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
docker run -v $PWD:/host -it --rm hepstore/rivet-pythia
```

Move to the `/host` directory and write some output to a file

```bash
cd /host
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



<!-- ### Challenge 3

Copy an example code `main01.cc` and `Makefile*` from `/usr/local/share/Pythia8/examples/` of the container to your shared area. Compile the code with `make`and run it.

:::::::::::::::: solution

Start the root container with

```bash
docker run -v $PWD:/host -it --rm hepstore/rivet-pythia
```

Move to the shared area

```bash
cd /host
```

Copy the files with

```bash
cp /usr/local/share/Pythia8/examples/main01.cc .
cp /usr/local/share/Pythia8/examples/Makefile* .
```

Compile and run

```bash
make main01.cc & ./main01
```

FIXME: this does not work

:::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::: -->

::::::::::::::::::::::::::::::::::::: keypoints 

- You have now set up a docker container as a working enviroment Rivet and Pythia.
- You know how to share a working area between your own computer and the container.

::::::::::::::::::::::::::::::::::::::::::::::::
