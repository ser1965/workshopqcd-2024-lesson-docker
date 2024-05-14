---
title: "Docker containers for CMS open data"
teaching: 15
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- What container images are available for my work with the CMS open data?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Download the ROOT and python images and build your own container
- Learn how to share a working area between your laptop and the container image.
- Restart an existing container
- Delete and rebuild containers

::::::::::::::::::::::::::::::::::::::::::::::::

## Overview

This exercise will walk you through setting up and familiarizing yourself with Docker, so that
you can effectively use it to interface with the CMS open data. It is *not* meant to completely
cover containers and everything you can do with Docker. <!--- Reach out to the organizers
using the [dedicated Mattermost channel][mattermost]
if we are missing something. -->

Three types of container images are provided: [one with the CMS software (CMSSW)](https://gitlab.cern.ch/cms-cloud/cmssw-docker-opendata/-/blob/master/README.md) compatible with the released data, and two others with [ROOT](https://gitlab.cern.ch/cms-cloud/root-vnc) and [python](https://gitlab.cern.ch/cms-cloud/python-vnc) libraries needed in this workshop. 

You will only need the CMSSW container, if you want to access the CMS data in the MiniAOD format (you will learn about them later). Access to it requires CMSSW software that you will not be able to install on your own computer. 

During the workshop hands-on lessons, we will be using the CMS data the NanoAOD format and access to it does not require any CMS-specifc software. Two containers are provided to make setting up and using ROOT and/or python libraries easier for you for this tutorial, but if you wish, you can also install them on your computer. 

All container images come with [VNC](https://gitlab.cern.ch/cms-cloud/cmssw-docker-opendata/-/blob/master/README.md#use-vnc) for the graphical use interface. It opens directly in a browser window. Optionally, you can also connect to the VNC server of the container using a VNC viewer (VNC viewer (TigerVNC, RealVNC, TightVNC, OSX built-in VNC viewer, etc.) installed on your local machine, but only the browser option for which no additional tools are needed is described in these instructions. On native Linux, you can also use X11-forwarding.

For different container images, some guidance can be found on the
[Open Data Portal introduction to Docker](http://opendata.cern.ch/docs/cms-guide-docker). The use of graphical interfaces, such the graphics window from ROOT, depends on the operating system of your computer. Therefore, in the following, separate instructions are given for Windows WSL, Linux and MacOS.

## Start the container

The first time you start a container, a docker image file gets downloaded from an image registry. The open data images are large (2-3GB for the root and python tools images and substantially larger for the CMSSW image) and it may take long to download, depending on the speed of your internet connection. After the download, a container created from that image starts. The image download needs to be done only once. Afterwards, when starting a container, it will find the downloaded image on your computer, and it will be much faster.

The containers do not have modern editors and it is expected that you mount your working directory from the local computer into the container, and use your normal editor for editing the files. Note that all your compiling and executing still has to be done in the Docker container!

First, before you start up your container, create a local directory where you will be doing your code development. In the examples below, it is called `cms_open_data_python` and `cms_open_data_root`, respectively, and the variable will record your working directory path. You may choose a different location and a shorter directory name if you like.

### Python tools container

Create the shared directory in your working area:

```bash
export workpath=$PWD
mkdir cms_open_data_work
chmod -R 777 cms_open_data_python
```

Start the container with

```bash
docker run -it --name my_python -P -p 8888:8888 -v ${workpath}/cms_open_data_python:/code gitlab-registry.cern.ch/cms-cloud/python-vnc:python3.10.5

```

You will get a container prompt similar this:

```output
cmsusr@4fa5ac484d6f:/code$
```

This is a bash shell and you can see the files in your shared area.

You can now open a jupyter lab from the container prompt with

```bash
jupyter-lab --ip=0.0.0.0 --no-browser
```

and open the link that is printed out in the message.

Click on the Jupyter notebook icon to open a new notebook. 


:::::::::::::::::::::::::::::: callout

## Permission denied?

If a window with "Error Permission denied: Untitled.ipynb" pops up, you most likely forgot to define the path variable, to create the shared directory or to change its permissions. Exit the jupyter lab with Control-C and confirm with y, then type `exit` to stop the container. Remove the container with

```bash
docker rm my_python
```

and now start all over from [the start](#python-tools-container).

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

### Root tools container

Create the shared directory in your working area:

```bash
export workpath=$PWD
mkdir cms_open_data_work
chmod -R 777 cms_open_data_root
```

Then start the container:

::: tab

### Windows WSL, Mac and Linux without X11-forwarding

```bash
docker run -it --name my_root -P -p 5901:5901 -p 6080:6080 -v ${workpath}/cms_open_data_root:/code gitlab-registry.cern.ch/cms-cloud/root-vnc:latest
```

For graphics, use VNC that is installed in the container and start the graphics windows with `start_vnc`. Open the browser window in the address given at the start message (http://127.0.0.1:6080/vnc.html) with the default VNC password is `cms.cern`. It shows an empty screen to start with and all graphics will pop up there.

Type `exit` to leave the container, and if you have started VNC, stop it first:

```bash
stop_vnc
exit
```

### Linux using X11-forwarding

```bash
docker run -it --name my_root --net=host --env="DISPLAY" -v $HOME/.Xauthority:/home/cmsusr/.Xauthority:rw -v ${workpath}/cms_open_data_root:/code gitlab-registry.cern.ch/cms-cloud/root-vnc:latest
```

For graphics, X11-forwarding to your host is used.

:::




::::::::::::::::::::::::::::::::::::: keypoints 

- You have now set up a docker container as a working enviroment for CMS open data.
- You know how to pass files between your own computer and the container.
- You know how to open a graphical window of ROOT or a jupyterlab in your browser using software installed in the container.

::::::::::::::::::::::::::::::::::::::::::::::::
