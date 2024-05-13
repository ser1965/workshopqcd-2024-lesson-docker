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

We will be using the CMS data the NanoAOD format and access to it does not require any CMS-specifc software. The two containers are provided to make setting up and using ROOT and/or python libraries easier for you for this tutorial, but if you wish, you can also install them on your computer. 

All container images come with [VNC](https://gitlab.cern.ch/cms-cloud/cmssw-docker-opendata/-/blob/master/README.md#use-vnc) for the graphical use interface. It opens directly in a browser window. Optionally, you can also connect to the VNC server of the container using a VNC viewer (VNC viewer (TigerVNC, RealVNC, TightVNC, OSX built-in VNC viewer, etc.) installed on your local machine, but only the browser option for which no additional tools are needed is described in these instructions. On native Linux, you can also use X11-forwarding.

For different container images, some guidance can be found on the
[Open Data Portal introduction to Docker](http://opendata.cern.ch/docs/cms-guide-docker). The use of graphical interfaces, such the graphics window from ROOT, depends on the operating system of your computer. Therefore, in the following, separate instructions are given for Windows WSL, Linux and MacOS.

## Start the container

The first time you start a container, a docker image file gets downloaded from an image registry. The open data images are large (2-3GB for the root and python tools images and substantially larger for the CMSSW image) and it may take long to download, depending on the speed of your internet connection. After the download, a container created from that image starts. The image download needs to be done only once. Afterwards, when starting a container, it will find the downloaded image on your computer, and it will be much faster.

The containers do not have modern editors and it is expected that you mount your working directory from the local computer into the container, and use your normal editor for editing the files. Note that all your compiling and executing still has to be done in the Docker container!

First, before you start up your container, create a local directory where you will be doing your code development. In the example below, it is called cms_open_data_work and it will live in the $HOME directory. You may choose a different location and a shorter directory name if you like.

### Python tools container


::: group-tab

### Windows WSL

Run ...

### Mac

Run ...

### Linux

X11 

:::

### Root tools container

::: group-tab

### Windows WSL

4

### Mac

5

### Linux

6

:::







::::::::::::::::::::::::::::::::::::: keypoints 

- You have now set up a docker container as a working enviroment for CMS open data.
- You know how to pass files between your own computer and the container.
- You know how to open a graphical window of ROOT or a jupyterlab in your browser using software installed in the container.

::::::::::::::::::::::::::::::::::::::::::::::::
