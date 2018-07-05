

build-ros2-lgsvl
================

## Video

[![ROS2 on webOS: Web-app enabled robots](http://img.youtube.com/vi/Ipl7TLSQfwI/0.jpg)](http://www.youtube.com/watch?v=Ipl7TLSQfwI)



## Introduction

### Description

This repository contains the top level code that aggregates the various [OpenEmbedded](http://openembedded.org), webOS OSE, and ROS2 layers into a whole from which robot images can be built. Cloning just this repo will allow building ROS2 LGSVL robot images on webOS OSE (Open Source Edition). The Yocto layers built by this project are:

* [oe-core](https://github.com/openembedded/openembedded-core)
* [meta-oe](https://github.com/openembedded/meta-openembedded)
* [meta-qt5](https://github.com/meta-qt5/meta-qt5)
* [meta-webos](https://github.com/webosose/meta-webosose)
* [meta-raspberrypi](http://git.yoctoproject.org/cgit.cgi/meta-raspberrypi)
* [meta-ros2](https://github.com/lgsvl/meta-ros2)
* [meta-ros2-lgsvl](https://github.com/lgsvl/meta-ros2-lgsvl)

### About the Project

This project is an initial demonstration of running ROS2 on webOS. It pulls together several open source projects in a physical prototype that demonstrates the advantages and potential of running ROS2 on webOS, including web application development using the [Enact](https://enactjs.com/) framework. 

The hardware setup includes a Raspberry Pi 3 with a motor shield and [Intel Neural Compute Stick](https://developer.movidius.com/) attached for deep learning. The project supports the Raspberry Pi camera (python); I2C, GPIO, and SPI drivers; and the official Raspberry Pi Touch Display.

Currently, the ROS2 packages that are built (meta-ros2-lgsvl) are a port of the following packages originally from the [Duckietown](http://duckietown.org/) project.

This build is currently based on the Ardent Apalone release of ROS2. There are plans to update to the latest release, as well as to have an automated generation process for meta-ros2 that is always up-to-date.

### What You May Find Useful

With this project, you can either try building the entire image as we provide it to build your own Beanbird robot, or take specific layers and components to build your own Yocto-based distribution running ROS/ROS2, with your own ROS packages.	



## Instructions

### Cloning

Set up build-ros2-lgsvl by cloning its Git repository:

     git clone https://github.com/lgsvl/build-ros2-lgsvl.git

Note: If you populate it by downloading an archive (zip or tar.gz file), then you will get the following error when you run mcf:

     fatal: Not a git repository (or any parent up to mount parent).
     Stopping at filesystem boundary (GIT_DISCOVERY_ACROSS_FILESYTEM not set).


### Prerequisites
Before you can build, you will need some tools.  If you try to build without them, bitbake will fail a sanity check and tell you what's missing, but not really how to get the missing pieces. On Ubuntu, you can force all of the missing pieces to be installed by entering:

    $ sudo scripts/prerequisites.sh

Also, the bitbake sanity check will issue a warning if you're not running under Ubuntu 14.04 64bit LTS.


### Building
To configure the build for the raspberrypi3 and to fetch the sources:

    $ ./mcf -p 0 -b 0 raspberrypi3

The `-p 0` and `-b 0` options set the make and bitbake parallelism values to the number of CPU cores found on your computer.

To kick off a full build of webOS OSE, make sure you have at least 100GB of disk space available and enter the following:

    $ make webos-image

This may take in the neighborhood of two hours on a multi-core workstation with a fast disk subsystem and lots of memory, or many more hours on a laptop with less memory and slower disks or in a VM.


### Images
The following images can be built:

- `webos-image`: The production webOS OSE image.
- `webos-image-devel`: Adds various development tools to `webos-image`, including gdb and strace. See `packagegroup-core-tools-debug` and `packagegroup-core-tools-profile` in `oe-core` and `packagegroup-webos-test` in `meta-webos` for the complete list.


### Cleaning
To blow away the build artifacts and prepare to do clean build, you can remove the build directory and recreate it by typing:

    $ rm -rf BUILD
    $ ./mcf.status

What this retains are the caches of downloaded source (under `./downloads`) and shared state (under `./sstate-cache`). These caches will save you a tremendous amount of time during development as they facilitate incremental builds, but can cause seemingly inexplicable behavior when corrupted. If you experience strangeness, use the command presented below to remove the shared state of suspicious components. In extreme cases, you may need to remove the entire shared state cache. See [here](https://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html#shared-state-cache) for more information on it.


### Building Individual Components
To build an individual component, enter:

    $ make <component-name>

To clean a component's build artifacts under BUILD, enter:

    $ make clean-<component-name>

To remove the shared state for a component as well as its build artifacts to ensure it gets rebuilt afresh from its source, enter:

    $ make cleanall-<component-name>

### Adding new layers
The script automates the process of adding new OE layers to the build environment.  The information required for integrate new layer are; layer name, OE priority, repository, identification in the form branch, commit or tag ids. It is also possible to reference a layer from local storage area.  The details are documented in weboslayers.py.



Copyright and License Information
---------------------------------
Unless otherwise specified, all content, including all source code files and
documentation files in this repository are:

Copyright (c) 2008-2018 LG Electronics, Inc.

Unless otherwise specified or set forth in the NOTICE file, all content,
including all source code files and documentation files in this repository are:
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this content except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
