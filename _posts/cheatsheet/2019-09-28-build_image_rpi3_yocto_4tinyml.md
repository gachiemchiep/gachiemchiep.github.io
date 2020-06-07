---
title: "Build image for using in tinyml"
excerpt: "How to build image for raspberry pi using yocto project."
categories: 
  - cheatsheet
tags: 
  - yocto
published: true
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---

# TODO

Huhm we should change everything into Dunfell and test again

https://wiki.yoctoproject.org/wiki/Releases

# Summary

一覧

1. [build_image_rpi3_yocto/](/cheatsheet/build_image_rpi3_yocto/)
2. [add ml things](/_pages/cheatsheet/build_image_rpi3_yocto_4tinyml/)

Read tinyml book
Need some customized image to do all the examples

https://gachiemchiep.github.io//cheatsheet/build_image_rpi3_yocto/

Addition layer
https://github.com/renesas-rz/meta-renesas-ai

```bash
# List recipes
bitbake -g your-image-name

# meta-python2
https://git.openembedded.org/meta-python2
```



