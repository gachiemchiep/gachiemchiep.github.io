---
title: "Introduction to Building a Machine Vision Inspection."
excerpt: "Introduction to Building a Machine Vision Inspection."
categories: 
  - news
tags: 
  - adasky
published: false
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---

# Summary

[book link](https://www.amazon.com/Handbook-Machine-Computer-Vision-Developers/dp/3527413391)

A quick summary of chapter 2 of [Handbook of Machine and Computer Vision: The Guide for Developers and Users](https://www.amazon.com/Handbook-Machine-Computer-Vision-Developers/dp/3527413391)

$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$


\\[ x = {-b \pm \sqrt{b^2-4ac} \over 2a} \\]



The sequence of a project realization can be seen as follows

1. specification of  the task
2. design the system
3. calculation of costs
4. development and installation of the system

Steps for designing machine vision inspection

1. Choosing the camera scan type
2. Determining the field of view
3. calculating the resolution
4. choosing a lens
5. selecting a camera model, frame grabber, hardware platform
6. selecting the illumination
7. addressing the aspects of mechanical and electrical interfaces
8. designing and choosing the software


# Detail

## 1. Choosing the camera scan type

1. line scan cameras
    *  offer higher resolution in both cross direction and scan direction
    * Good for processing continous image data
    * more expensive than area camera

![line scan cameras direction](/assets/images/2020/HMCV/0001.png)


2. area camera : global shutter
3. area camera : rolling shutter

TODO

## 2. Determining the field of view

The field of view is determined by the following factors: 

* maximum part size
* maximum variation of part presentation in translation
* margin as an offset to part size
* aspect ratio of the camera sensor 

![camera sensor size](https://assets.newatlas.com/dims4/default/e9adccf/2147483647/strip/true/crop/2000x1484+0+0/resize/1294x960!/format/webp/quality/90/?url=http%3A%2F%2Fnewatlas-brightspot.s3.amazonaws.com%2Farchive%2Fcamera-sensor-size-12.jpg)

\\[ FOV = maximum_part_size + tolerance_in_positioning + margin + adaption_to_the_aspect_ratio_of_the_camera_sensor \\]

In a short word, we want the inspected object fit inside captured image in all cases and conditions.

![FOV](/assets/images/2020/HMCV/0002.png)


## 3. calculating the resolution

The following distinction is necessary:

* camera sensor resolution 
* spatial resolution
* measurement accuracy


![camera sensor resolution](https://www.vision-doctor.com/images/stories/kamera/grundlagen/sensor_and_graphic_display_resolutions.jpg)
Source: [https://www.vision-doctor.com/en/camera-technology-basics/resolution-of-sensors.html](https://www.vision-doctor.com/en/camera-technology-basics/resolution-of-sensors.html)

![camera sensor resolution](https://upload.wikimedia.org/wikipedia/commons/f/f2/Resolution_illustration.png)
camera sensor resolution = number of total pixels, higher resolution is better

* spatial resolution refers to the imaging modality to differentiate two objects.

![spatial resolution](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Matakis_-_blurred.jpg/400px-Matakis_-_blurred.jpg)
Low spatial resolution

![High spatial resolution](https://upload.wikimedia.org/wikipedia/commons/thumb/1/17/MARTAKIS1.jpg/400px-MARTAKIS1.jpg)
High spatial resolution

See [wikipedia](https://en.wikipedia.org/wiki/Image_resolution), for a full description.

Measurement accuracy is the smallest feature that can be use to measure

| Algorithm        | Accuracy in pixel |
|------------------|:-----------------:|
| Edge detection   |        1/3        |
| Blog             |         3         |
| Pattern matching |         1         |

Then the necessary resolution can be calculated as: 

![cal_1](/assets/images/2020/HMCV/0003.png)

![cal_2](/assets/images/2020/HMCV/0003.png)

Personally, this is the least important factor. 
In the beginning, it is safer to choose a high resolution camera to work with.

## 4. choosing a lens
## 5. selecting a camera model, frame grabber, hardware platform

Not important at all

![bandwidth](/assets/images/2020/HMCV/0005.png)


## 6. selecting the illumination
## 7. addressing the aspects of mechanical and electrical interfaces
## 8. designing and choosing the software
