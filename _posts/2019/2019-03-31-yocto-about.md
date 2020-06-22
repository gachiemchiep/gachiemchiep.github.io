---
title: "A quick glance at yocto"
excerpt: "yocto summary."
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

# A quick glance at yocto

## What is yocto ?

[yocto](https://www.yoctoproject.org/) is a linux image build system. Which mainly be used to build embedded linux distribution. 

The core components of the yocto are :

1. Build engine : [bibake](https://github.com/openembedded/bitbake) which used to compile package and build the distribution
2. Configuration layers : [OpenEmbedded-Core](https://github.com/openembedded/openembedded-core) which used to guide the Build engine how to compile each package
3. Reference system : [poky](https://www.yoctoproject.org/software-item/poky/) which contains build engine, configuration layers and other useful tools/

## Why we need yocto 

Linux is the dominating operating systems in the embedded markets. A survey by [AspenCore in 2017](https://m.eet.com/media/1246048/2017-embedded-market-study.pdf) show the un-suprisingly result. 
Linux is great for soft-realtime but for hard-realtime operating system, Linux is not that strong 
compare to RTOS. This is the result why there're a big pie in embedded markets occupied by RTOS.

![2017-linux-os-share](/assets/images/yocto/embedded-market-os-share.png)

When we develop embedded system, we always face the followings problems. 
This result in a neccesary of customizing the OS of our embedded system.

1. We want to add new drivers
2. We want to compile some piece of software using prioerity toolchain.
3. We want to cut off unescessary things
4. We want to add some firmware-on-the-air
5. We want to control everything

Customizing the OS of embedded system could be done in 3 ways:

2. **Building everything by hand**
1. **Using binary image supplied by hardware-maker** 
2. **Using build system**

**Using binary image supplied by hardware-maker** is hard to customize and come in very big 
filesystem size. **Building everything by hand** is very messy and will cost tons of time and effort.
Compare with those two approach, **using yocto** will have the followings pros. (Source : [slide](https://bootlin.com/doc/training/yocto/yocto-slides.pdf))

![yocto-pros](/assets/images/yocto/yocto-pros.png)

## Conclusion

yocto is a very large projects and contains [tons of manual and documents](https://www.yoctoproject.org/docs/). Which is very detail and very difficult to follow.

I personally think that the best way to learn yocto is through practice. 
So i make some yocto tutorial here. First to summarize my know-how in using yocto, 
Secondly hope that this tutorial could help somebody. So in the next post, i will go into further detail about yocto and how to use it.

Thank you.


