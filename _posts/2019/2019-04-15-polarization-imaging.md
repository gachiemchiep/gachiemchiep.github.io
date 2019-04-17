---
title: "Polarization imaging"
excerpt: "Some notes about Polarization imaging."
categories: 
  - vision
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

- [Summary](#summary)
  - [Challenges in inspection tasks](#challenges-in-inspection-tasks)
    - [**Why these inspection tasks are challenged ?**](#why-these-inspection-tasks-are-challenged)
    - [**What could go wrong here?**.](#what-could-go-wrong-here)
    - [**How can we solve this problem?**.](#how-can-we-solve-this-problem)
  - [Polarization Imaging](#polarization-imaging)
    - [Polarization imaging usage](#polarization-imaging-usage)
    - [Polarization theory](#polarization-theory)
    - [**How to apply polarization imaging?**](#how-to-apply-polarization-imaging)
      - [Polarizing lense](#polarizing-lense)
    - [polarization sensor](#polarization-sensor)
  - [Conclusion](#conclusion)
  - [Reference](#reference)

# Summary

## Challenges in inspection tasks

There're challenge inspection tasks that a normal CMOS/CCD camera can not solve. Those challenge inspection tasks can be divided into 3 categories as below . [Image source](https://d1d1c1tnh6i0t6.cloudfront.net/wp-content/uploads/2018/11/Going-Polarized-Presentation.pdf) And [Sony video](https://youtu.be/C5hhKcMYRGI?t=27)

1. Hidden stress in meterials
2. Strong reflective surfaces
3. Low contrast objects/surfaces

![Challenge tasks](/assets/images/2019/polarization_tasks_1.png)

### **Why these inspection tasks are challenged ?**

To answer this question, at first let's take a look at how a digital camera capture image. 

Inside every digital camera, there's a electronic equipment that captures the incoming light rays and turns them into electrical signals. This light detector is one of two types, either a charge-coupled device (CCD) or a CMOS image sensor. The electrical signals are later convert into data and can be stream or stored as digital image.

So *How can we capture image of inspected object ?*. We point the camera into inspected object, then push the button. The camera will capture light ray came from inspected object. And, we'll have a nice picture of inspected object. Such as the image below.

![Taking image](/assets/images/2019/image_focus.jpg)

### **What could go wrong here?**.

 When light come to the surface of inspected object, it will be reflected/absorbed/refracted. If the light is **reflected too much** or **absorbed/refracted too much**, the surface detail of inspected object will be lost. Inspected object appeared on captured image will be flared or low in contrast. This will make image analysing become nearly impossible. You can see a list of image on [link1](http://image-sensors-world.blogspot.com/2019/04/lucid-vision-on-polarization-camera-use.html) and [link2](https://www.sony-semicon.co.jp/products_en/IS/sensor5/index.html) for a list of image. 

![Challenge tasks](/assets/images/2019/polarization_tasks_1.png)

### **How can we solve this problem?**.

At this point, we can see that in those challenge inspectation task, surface detail of inspected object are flared or low in contrast. So we need to increase the surface detail of inspected object or reduce the light noise to capture better image.

There're several approaches that could be used for this situation. Such as hyperspectral camera (**increase information**), 3d camera (**increase information**), polarization imaging (**reduce nosie**), etc. In the scope of this post, i will talk only about polarization imaging.

## Polarization Imaging

Polarization imaging is a imaging technque that capture only certain part of input light ray. In the below image: the right images are captured by using polarization imaging. This cut of the flare inside image and capture a better and more vivid image. [source](https://www.oulu.fi/sites/default/files/206/Borovkova_Birefringence.pdf)

![Polarization Imaging](/assets/images/2019/polarization_ex1.png)

In photography, it's very popular to use a polarizing filter to reduce flaring from sky/sea/water picture. [source](https://www.motherjones.com/kevin-drum/2018/03/582175/)

![Photography](/assets/images/2019/blog_polarizing_lake_glare.jpg)

### Polarization imaging usage

Polarization imaging can be used for belows tasks. [Source: lucid vision lab](https://thinklucid.com/tech-briefs/polarization-explained-sony-polarized-sensor/)

1. Stress Inspection
2. Reduce Reflection
3. Improving Contrast
4. Scratch inspection
5. Object detection

### Polarization theory

Light have **Wave** and **Particle** properties. Most sources of light are classified as incoherent and unpolarized (or only "partially polarized") because they consist of a random mixture of waves having different spatial characteristics, frequencies (wavelengths), phases, and polarization states [source](https://en.wikipedia.org/wiki/Polarization_(waves)#Wave_propagation_and_polarization) . 

Unpolarized light is un-consistent and noisy. So we need to convert it into polarized light to haverst consistent and stable light ray. The below figure simply illustrate the difference between unpolarized light and polarized light [source: quora](https://www.quora.com/What-is-polarization-of-light) that illustrate the difference between unpolarized and polarized light.

![Challenge tasks](/assets/images/2019/polarization_light.png)

**How to convert unpolarized light into polarized light ?**. Unpolarized light can be converted into polarized light by using light reflection or refraction effect. For example, in the below image, the red part of light are extracted and become polarized light [source: lucid vision labs](https://thinklucid.com/tech-briefs/polarization-explained-sony-polarized-sensor/) . 

![convert unpolarized light into polarized light ](/assets/images/2019/reflection-refraction-polarization.gif)

### **How to apply polarization imaging?**

There're 2 approaches can be used to apply polarization imaging.

1. polarizing lense
2. polarization camera/sensor

#### Polarizing lense

Tradition system use polarizing lense. The lenses are placed in front of camera so part of in-come light ray is filtered and result in more contrasted and clear image. This approach have 2 disadvantages as below.

1. Expensive and hard to mainternance
2. Can not completely reduce noise

Several lenses need to be integrated into vision system. This increase the total cost and number of equipments. Finally results is a vision system that's expensive and hard to mainternance

![Photography](/assets/images/2019/polarization_system_1.png)

There's rare case as the below image. In this case, a small part of light ray are refracted into the nearby camera. **Personally, i haven't meet this problem yet, but a problem is a problem so i list it here.**

![Photography](/assets/images/2019/Polarization-Sensor-Crosstalk-1.gif)

### polarization sensor

In 2018, Sony introduce the first ever polarization sensor **IMX250MZR**  [source](https://www.ptgrey.com/sony-polarization). This is the first polarization sensor in the vision industry. So from now on, we'll have another choice to work with polarization imaging.

**IMX250MZR** sensor has the following structure :

![structure](/assets/images/2019/polarization_sensor_structure.png)

Each pixel has its own polarization filter. These filters are oriented at 0 °, 45 °, 90 °, 135 ° and are arranged to repeat the 2-pixel block.
The polarization filter (C) for each pixel is coated with an antireflective layer (B) and placed between the microlens (A) and the photosensitive photodiode (E).

Comparing with polarization lense, polarization sensor could completely remove noise.

![Photography](/assets/images/2019/polarization-sensor-no-crosstalk-2.gif)

These below picture are capture by using **IMX250MZR** sensor. Personally, it's quite impressive result [source](https://www.ptgrey.com/sony-polarization).

![Photography](/assets/images/2019/polarization_sensor_img1.png)

![Photography](/assets/images/2019/polarization_sensor_img2.png)

![Photography](/assets/images/2019/polarization_sensor_img3.png)

## Conclusion

1. Polarizating imaging is still a new type of sensor. More time is needed until this sensor is mature enough. But it's a new cool sensor for computer vision engineer.

## Reference

1. [introduction-to-polarization/](https://www.edmundoptics.com/resources/application-notes/optics/introduction-to-polarization/)
2. [polarization application](https://www.edmundoptics.com/resources/application-notes/illumination/successful-light-polarization-techniques/)
3. [quora](https://www.quora.com/What-is-polarization-of-light)
4. [sony-polarization sensor](https://www.ptgrey.com/sony-polarization)
5. [phan cuc la gi](https://vi.wikipedia.org/wiki/Ph%C3%A2n_c%E1%BB%B1c)
6. [lucid vision : explained-sony-polarized-sensor](https://thinklucid.com/tech-briefs/polarization-explained-sony-polarized-sensor/)
7. [Going-Polarized-Presentation](https://d1d1c1tnh6i0t6.cloudfront.net/wp-content/uploads/2018/11/Going-Polarized-Presentation.pdf)
8. [use case cua polaration imaging](http://image-sensors-world.blogspot.com/2019/04/lucid-vision-on-polarization-camera-use.html)
9. [08_Stage1_1430_LUCID](https://ibv.vdma.org/documents/256550/27019077/2018-11-08_Stage1_1430_LUCID.pdf/
)
10. [Lucid vision lab white paper]( https://dce9ugryut4ao.cloudfront.net/LUCID-Going-Polarized-White-Paper.pdf)
11. [sony polarization sensor](https://www.sony-semicon.co.jp/products_en/IS/sensor5/index.html)



