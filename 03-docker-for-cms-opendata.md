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
- Restart an existing container
- Learn how to share a working area between your laptop and the container image.

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
mkdir cms_open_data_python
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

This is a bash shell in the container. If you had some files in the shared area, they would be available here.

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

Close the jupyter lab with Control-C and confirm with y. Type `exit` to leave the container:

```bash
exit
```

### Root tools container

Create the shared directory in your working area:

```bash
export workpath=$PWD
mkdir cms_open_data_root
chmod -R 777 cms_open_data_root
```

Then start the container, depending on your host system:

:::::::::::::::: spoiler

### Windows WSL, Mac and Linux without X11-forwarding

```bash
docker run -it --name my_root -P -p 5901:5901 -p 6080:6080 -v ${workpath}/cms_open_data_root:/code gitlab-registry.cern.ch/cms-cloud/root-vnc:latest
```
You will get a container prompt similar this:

```output
cmsusr@9b182de87ffc:/code$
```


For graphics, use VNC that is installed in the container and start the graphics windows with `start_vnc`. Open the browser window in the address given at the start message (http://127.0.0.1:6080/vnc.html) with the default VNC password is `cms.cern`. It shows an empty screen to start with and all graphics will pop up there.

You can test it with ROOT:

```bash
start_vnc
root
```

Open a ROOT Object Browser by typing 

```
TBrowser t
```

in the ROOT prompt.

You should see it opening in the VNC tab of your browser.

Exit ROOT with

```
.q
```

in the ROOT prompt.

Type `exit` to leave the container, and if you have started VNC, stop it first:

```bash
stop_vnc
exit
```
:::::::::::::::: 

:::::::::::::::: spoiler

### Linux using X11-forwarding

```bash
docker run -it --name my_root --net=host --env="DISPLAY" -v $HOME/.Xauthority:/home/cmsusr/.Xauthority:rw -v ${workpath}/cms_open_data_root:/code gitlab-registry.cern.ch/cms-cloud/root-vnc:latest
```

For graphics, X11-forwarding to your host is used.

You can test it with ROOT:

```bash
root
```

Open a ROOT Object Browser by typing 

```
TBrowser t
```

in the ROOT prompt.

You should see the ROOT Object Broswer opening.

Exit ROOT with

```
.q
```

in the ROOT prompt.

Type `exit` to leave the container:

```bash
exit
```

::::::::::::::::::::

### Exercises

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1

Create a file on your local host and make sure that you can see it in the container. 

:::::::::::::::: solution

Open the editor in your host system, create a file `example.txt` and save it to the shared working area, either in `cms_open_data_python` or in `cms_open_data_root`.

Restart the container, making sure to choose the container that is connected to shared working area that you have chosen:

```bash
docker start -i my_python
```

or 

```bash
docker start -i my_root
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

Make a plot with the jypyter notebook, save it to a file and make sure that you get it to your host 

:::::::::::::::: solution

Restart the python container with

```bash
docker start -i my_python
```

Start the jypyter lab with

```bash
jupyter-lab --ip=0.0.0.0 --no-browser
```

Open a new jupyter notebook.

Make a plot of your choice. For a quick CMS open data plot, you can use the following:

```python
import uproot
import matplotlib.pylab as plt
import awkward as ak
import numpy as np

# open a CMS open data file, we'll see later how to find them
file = uproot.open("root://eospublic.cern.ch//eos/opendata/cms/Run2016H/SingleMuon/NANOAOD/UL2016_MiniAODv2_NanoAODv9-v1/120000/61FC1E38-F75C-6B44-AD19-A9894155874E.root")
file.classnames() # this is just to see the top-level content
events = file['Events']
# take all muon pt values in an array, we'll see later how to find the variable names
pt = events['Muon_pt'].array() 
plt.figure()
plt.hist(ak.flatten(pt),bins=200,range=(0,200))
plt.show()
plt.savefig("pt.png") 
```

Run the code shell, and you should see the muon pt plot.

You can rename your notebook by right-clicking on the name in the left bar and choosing "Rename".

In a shell on your host system, move to the working directory, list the files and you should see the notebook and the plot file.

```bash
ls
```

```output
myplot.ipynb  pt.png
```



:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge


### Challenge 3

Make a plot with ROOT, save it to a file and make sure that you get it to your host 

:::::::::::::::: solution

Restart the root container with

```bash
docker start -i my_root
```

If you are using VNC, start it in the container prompt with

```bash
start_vnc
```

Open the URL in your browser and connect with the password `cms.cern`.

Start ROOT and make a plot of your choice. 

For a quick CMS open data plot, you can open a CMS open data file with ROOT with

```bash
 root root://eospublic.cern.ch//eos/opendata/cms/Run2016H/SingleMuon/NANOAOD/UL2016_MiniAODv2_NanoAODv9-v1/120000/61FC1E38-F75C-6B44-AD19-A9894155874E.root
```

In the ROOT prompt, type

```
TBrowser t
```

to open the ROOT object browser, which opens in your broswer VNC tab. Double click on the file name, then on `Events` and then a variable of your choice, e.g. `nMuon`

You should see the plot. To save it, right click in the plot margins and you will see a menu named `TCanvas::Canvas_1`. Choose "Save as" and give the name, e.g. `nmuon.png`.

Quit ROOT by typing `.q` on the ROOT prompt or choosing "Quit Root" from the ROOT Object Browser menu options.

In a terminal on your host system, move to the working directory, list the files and you should see the plot file.

```bash
ls
```

```output
nmuon.png
```



:::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 

- You have now set up a docker container as a working enviroment for CMS open data.
- You know how to pass files between your own computer and the container.
- You know how to open a graphical window of ROOT or a jupyterlab in your browser using software installed in the container.

::::::::::::::::::::::::::::::::::::::::::::::::
