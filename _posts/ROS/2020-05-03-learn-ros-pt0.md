---
title: "ROS講座"
excerpt: "Some quick notes when learning ROS."
categories: 
  - ROS
tags: 
  - tutorial
  - TODO
comments: true
published: false
toc: true
support: true
order: 9
author: vugia.truong
---

# Learn ROS

This is the best tutorial so far
-> can copy markdown and post and self-blog -> could take around 1 month
-> on let's read first

## ROS講座01 コンセプト
https://qiita.com/srs/items/d7c7202236d2abe562c8

Install : simple 

build : catkin = cmake build system, can link other library

runtime: 

```bash
publish/subcribe model : http://itdoc.hitachi.co.jp/manuals/link/cosmi_v0870/APKC/EU070377.HTM
a lot of robot tools : rviz, gazebo, rqt
roslaunch
```

```bash
git clone https://github.com/project-srs/ros_lecture.git
sudo apt-get install -y libarmadillo-dev libarmadillo6 
git clone https://github.com/tysik/obstacle_detector.git
mv obstacle_detector/ src
sudo apt install ros-melodic-jsk-rviz-plugins
catkin_make
```

# Reference

1. [ROS講座](https://qiita.com/srs/items/5f44440afea0eb616b4a)


以上