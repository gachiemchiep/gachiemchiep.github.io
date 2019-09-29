---
title: "Build image for raspberry pi using yocto"
excerpt: "How to build image for raspberry pi using yocto project."
categories: 
  - cheatsheet
tags: 
  - yocto
  - TODO
published: true
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---


# Summary

Yocto is a great tool to build linux-image for device. 
In this post i will summarize the procedure needed to build linux-image 
for **raspberry pi B+** using yocto.

The following 2 types of disk image will be built:

1. CUI : core-image-basic
2. GUI X11 : core-image-sato

Addition software for the image

1. OpenCV
2. librealsense with python support

Build environment

| Tables   |      Detail      |  
|----------|:-------------:|
| OS |  Ubuntu-18.04 LTS | 
| Processor |  AMD   |   
| yocto | poky-warrior (2.7.1) |    


# Detail

The procedure is as followed

1. Prepare the build environment
2. Build the linux-image
3. Results

## 1. Prepare the build environment

* Prepare the build environment

```bash

# Essential packages
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
     build-essential chrpath socat

# Download yocto
mkdir ~/tools
cd ~/tools
git clone -b warrior git://git.yoctoproject.org/poky.git poky-warrior

# Get useful layers
cd ~/tools/poky-warrior
mkdir layers
cd layers

git clone git://git.openembedded.org/meta-openembedded
# raspberry
git clone git://git.yoctoproject.org/meta-raspberrypi
# 96boards
git clone git://github.com/96boards/meta-96boards
# realsense
git clone https://github.com/IntelRealSense/meta-intel-realsense.git

source poky/oe-init-build-env rpi64-build


```

## 2. Build the linux-image

* Create the build directory

```bash
mkdir -p ~/workspace/poky-build
cd ~/workspace/poky-build

# Create the build directory inside rpi64-build
source ~/tools/poky-warrior/oe-init-build-env rpi64-build

```

* Add the layers to **conf/bblayers.conf** . the file content is as followed

```bash
POKY_BBLAYERS_CONF_VERSION = "2"

BBPATH = "${TOPDIR}"
BBFILES ?= ""

BBLAYERS ?= " \
  ${HOME}/tools/poky-warrior/meta \
  ${HOME}/tools/poky-warrior/meta-poky \
  ${HOME}/tools/poky-warrior/meta-yocto-bsp \
  ${HOME}/tools/poky-warrior/layers/meta-openembedded/meta-oe \
  ${HOME}/tools/poky-warrior/layers/meta-openembedded/meta-multimedia \
  ${HOME}/tools/poky-warrior/layers/meta-openembedded/meta-networking \
  ${HOME}/tools/poky-warrior/layers/meta-openembedded/meta-python \
  ${HOME}/tools/poky-warrior/layers/meta-openembedded/meta-perl \
  ${HOME}/tools/poky-warrior/layers/meta-openembedded/meta-gnome \
  ${HOME}/tools/poky-warrior/layers/meta-openembedded/meta-xfce \
  ${HOME}/tools/poky-warrior/layers/meta-raspberrypi \
  ${HOME}/tools/poky-warrior/layers/meta-96boards \
  ${HOME}/tools/poky-warrior/layers/meta-intel-realsense \
  "

```

* Add the following lines to **conf/local.conf**.

```bash
# Select machine
# See https://github.com/agherzan/meta-raspberrypi/tree/master/conf/machine 
# for a list of available machine
MACHINE = "raspberrypi3-64"

# Make faster
BB_NUMBER_THREADS = '6'
PARALLEL_MAKE = '-j 6'

# useful software
CORE_IMAGE_EXTRA_INSTALL += "htop iotop iperf3"

# OpenCV 
# NOTE: OPENCV_contrib is already include inside opencv package
# add libopencv-XXX to insert new module 
CORE_IMAGE_EXTRA_INSTALL += "opencv \
                            libopencv-core-dev \
                            libopencv-highgui-dev \
                            libopencv-imgproc-dev \ 
                            libopencv-objdetect-dev \
                            libopencv-ml-dev \
                            opencv-apps \
                            opencv-dev \
                            libopencv-xphoto \
                            opencv-dev "

# realsense : need x11
CORE_IMAGE_EXTRA_INSTALL += "librealsense2 librealsense2-tools"
CORE_IMAGE_EXTRA_INSTALL += "librealsense2-debug-tools"

# Image size will be extended to fill up the sdcard
CORE_IMAGE_EXTRA_INSTALL_append = " 96boards-tools "

# Fix bug: some package is licensed so it won't be built by default
LICENSE_FLAGS_WHITELIST = "commercial"

# Add new user : pi/raspberry
# change password of root into root
EXTRA_USERS_PARAMS = " useradd pi; \
                       usermod  -p 'raspberry' pi; \
                       usermod  -a -G sudo pi; \
                       usermod -P root root; "
```

* For intelrealsense: create **conf/auto.conf** with the content as followed

```bash
require include/intel-librealsense.inc

# Python 2.x
CORE_IMAGE_EXTRA_INSTALL += "python-pyrealsense2"

# Python 3.x
CORE_IMAGE_EXTRA_INSTALL += "python3-pyrealsense2"
```

* Start the build

```bash
# CUI image
bitake core-image-base

# GUI X11 image
bitbake core-image-sato

# GUI Wayland/Weston
# bitbake core-image-wayland
```

**Note:** The build will take hours to finish.

## 3. Results

The following image will be created

```bash
# CUI image
tmp/deploy/images/raspberrypi3-64/core-image-base-raspberrypi3-64-20190928135901.rootfs.rpi-sdimg

# GUI X11 image
tmp/deploy/images/raspberrypi3-64/core-image-sato-raspberrypi3-64-20190928134950.rootfs.rpi-sdimg

```

We then use dd command to wride sdimg file into sd card

```bash
sudo dd if=core-image-base-raspberrypi3-64-20190928135901.rootfs.rpi-sdimg of=/dev/sdX bs=4M status=progress
sync
```

### 3.1 CUI image result

It can boot into CUI

![CUI image 1](/assets/images/yocto/rpi_64_cui_1.jpg)

Opencv and pyrealsense was installed correctly

![CUI image 2](/assets/images/yocto/rpi_64_cui_2.jpg)

### 3.2 GUI X11 image

It can boot into GUI

![GUI image 1](/assets/images/yocto/rpi_64_gui_1.jpg)

# References

1. [Bake 64-bit raspberryPI3 images with Yocto/OpenEmbedded](https://himvis.com/bake-64-bit-raspberrypi3-images-with-yoctoopenembedded/)
2. [Yocto で RaspberryPi3 OS を3種類の方法で build する](https://qiita.com/naohikowatanabe/items/fb7abb2db601013b8e7a)
3. [YoctoProject Release Activity](https://wiki.yoctoproject.org/wiki/Releases)
4. [rpi-basic-image (CUI)](https://www.usagi1975.com/24mar172100/)
5. [core-image-sato (GUI, X11)](http://mickey-happygolucky.hatenablog.com/entry/2017/02/08/013946)
6. [core-image-weston (GUI, Wayland/Weston(desktop-shell))](http://mickey-happygolucky.hatenablog.com/entry/2017/05/31/011009)


End