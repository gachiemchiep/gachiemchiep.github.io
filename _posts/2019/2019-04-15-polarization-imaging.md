---
title: "Polarization imaging"
excerpt: "Some notes about Polarization imaging."
categories: 
  - vision
  - todo
tags: 
  - polarization
  - sensor
published: true
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---

# Summary

TODO :

- [ ] polarization sensor structure analysis
- [ ] More detail

## Challenges in inspection tasks

There're challenge inspection tasks that a normal camera can not solve. Such as: 

1. Hidden stress in meterials
2. Strong reflective surfaces
3. Low contrast objects/surfaces

![Challenge tasks](/assets/images/2019/polarization_tasks_1.png)

[Image source](https://d1d1c1tnh6i0t6.cloudfront.net/wp-content/uploads/2018/11/Going-Polarized-Presentation.pdf) .There're other examples at [Sony video](https://youtu.be/C5hhKcMYRGI?t=27)

**Why these examples are challenged ?** . When light come to the surface of inspected object, it will be reflected/absorbed/refracted. Camera capture the reflected light and create image of inspected object. The focus function help camera to ignore light which is not reflected from inspected object. So the inspected object could appear clearly and vividly such as the image below

![Taking image](/assets/images/2019/image_focus.jpg)

**What could go wrong here?**. Imaging a scene when the light come to the surface of inspected object, and it is **reflected too much** or **absorbed/refracted too much**. Inspected object appeared on captured image will be flared or law in contrast. See the image below :

![Challenge tasks](/assets/images/2019/polarization_tasks_1.png)

**How can we solve this problem?**. The main idea is using polarization filter or polarization sensor to remove part of light that is noisy.

## Unpolarized to Polarized

Light have **Wave** and **Particle** properties. In this post, unpolarized and polarized words described **particle** property of light. There's simple figure at [quora](https://www.quora.com/What-is-polarization-of-light) that illustrate the difference between unpolarized and polarized light.

![Challenge tasks](/assets/images/2019/polarization_light.png)

**How to convert unpolarized light into polarized light ?**. Unpolarized light can be converted into polarized light by reflection or refraction. Image source [lucid vision labs](https://thinklucid.com/tech-briefs/polarization-explained-sony-polarized-sensor/) 

![convert unpolarized light into polarized light ](/assets/images/2019/reflection-refraction-polarization.gif)

## Usage of polarization imaging

Polarization imaging can be used for belows tasks. [Source: lucid vision lab](https://thinklucid.com/tech-briefs/polarization-explained-sony-polarized-sensor/)

1. Stress Inspection
2. Reduce Reflection
3. Improving Contrast
4. Scratch inspection
5. Object detection

In photography, it's very popular to use a polarizing filter to reduce flaring from sky/sea/water picture. [source](https://www.motherjones.com/kevin-drum/2018/03/582175/)

![Photography](/assets/images/2019/blog_polarizing_lake_glare.jpg)

## How to apply polarization imaging

Applying polarization imaging require special camera, lense to capture polarization image. Tradition system/camera use polarizing filter. The filters are placed in front of camera so part of in-come light if filtered and result in more contrasted and clear image. This approach have 2 disadvantages :

1. Expensive and hard to mainternance

![Photography](/assets/images/2019/polarization_system_1.png)

2. Still have noise left

![Photography](/assets/images/2019/Polarization-Sensor-Crosstalk-1.gif)

To solve those above problems, Sony introduce the first ever polarization sensor in 2018 [source](https://www.ptgrey.com/sony-polarization) . This show the leading position that they have in vision industry.

**So how Sony's polarization sensor solve those 2 above disadvantages ?**

1. Polarizing filter is embedded directly inside vision sensor so we don't need separate filter.
2. polarizer is embedded below the micro lens so noise is completly separated.

![Photography](/assets/images/2019/Polarization-Sensor-Crosstalk-2.gif)

## Conclusion

1. Polarizating imaging is still a new type of sensor. More time is needed until this sensor is mature enough
2. Low resolution, low speed
3. Total new type of imaging system, so imaging analysis is difficult.  


## Reference

1. [introduction-to-polarization/](https://www.edmundoptics.com/resources/application-notes/optics/introduction-to-polarization/)
2. [polarization application](https://www.edmundoptics.com/resources/application-notes/illumination/successful-light-polarization-techniques/)
3. [quora](https://www.quora.com/What-is-polarization-of-light)
4. [sony-polarization sensor](https://www.ptgrey.com/sony-polarization)
5. [phan cuc la gi](https://vi.wikipedia.org/wiki/Ph%C3%A2n_c%E1%BB%B1c)
6. [lucid vision : gioi thieu ve anh sang phan cuc](https://thinklucid.com/tech-briefs/polarization-explained-sony-polarized-sensor/)
7. [Going-Polarized-Presentation](https://d1d1c1tnh6i0t6.cloudfront.net/wp-content/uploads/2018/11/Going-Polarized-Presentation.pdf)
8. [use case cua polaration imaging](http://image-sensors-world.blogspot.com/2019/04/lucid-vision-on-polarization-camera-use.html)
9. [08_Stage1_1430_LUCID](https://ibv.vdma.org/documents/256550/27019077/2018-11-08_Stage1_1430_LUCID.pdf/
)
10. [Lucid vision lab white paper]( https://dce9ugryut4ao.cloudfront.net/LUCID-Going-Polarized-White-Paper.pdf)



