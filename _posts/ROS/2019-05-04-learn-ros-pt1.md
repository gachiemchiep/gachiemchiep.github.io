---
title: "Learning ROS. Part 1"
excerpt: "Some quick notes when learning ROS."
categories: 
  - ROS
tags: 
  - tutorial
comments: true
published: true
toc: true
support: true
order: 9
author: vugia.truong
---

<!-- TOC -->

- [1. A Gentle introduction to ROS](#1-a-gentle-introduction-to-ros)
  - [1.1. Why ROS is needed ?](#11-why-ros-is-needed-)
  - [1.2. Install ROS](#12-install-ros)
  - [1.3. ROS's general concepts](#13-ross-general-concepts)
  - [1.4. Reference](#14-reference)

<!-- /TOC -->
  
# 1. A Gentle introduction to ROS

My background is Computer Vision and i love it. But recently, my work changed a little bit and now i have to work with both Computer Vision and Robotics. So i have to learn ROS to work with some kinds of Robot Arm and so on.

Personally i found the [ROS tutorial](http://wiki.ros.org/ROS/Tutorials) is flooded with information and very hard to follow. Beginner level came up with 20 small tutorial, each tutorial has length like several A4 pages. The worst part is there isn't any top-down architecture expaining. So learning will take weeks and kill beginner's patience very fast. 

I wrote down my learning experience here. First, to clarify and summarize what i learned. Secondly, hope it could help some-one. 

## 1.1. Why ROS is needed ? 

A quick read at [A Gentle introduction to ROS](https://www.amazon.co.jp/Gentle-Introduction-ROS-Jason-OKane/dp/1492143235) have me the following points :

```bash
ROS is an open-source, meta-operating system for your robot. 
It provides the services you would expect from an operating system, 
including hardware abstraction, low-level device control, 
implementation of commonly-used functionality, 
message-passing between processes, 
and package management. 
It also provides tools and libraries for obtaining, 
building, writing, and running code across multiple computer
```

So ROS is very good at:

1. Distributed computation: ROS can use computation resource very effective
2. Software reuse: write code once and run on every hardware that support ROS
3. Rapid testing: ROS can accelarate the testing process

As i known, currently nearly all of industrial robot maker are using different version of their own operating system. ROS can theorically replace that and make all robot talk in the same language. So it will for sure accelarate the deploying process of robot.

## 1.2. Install ROS

Just run the following command to install all the needed packages to use ROS. 

```bash
# get the repo list
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
sudo apt update
# Install newest ros version: melodic
sudo apt install ros-melodic-desktop-full build-essential
sudo rosdep init
rosdep update
# Load ros environment whenever new terminal is opened
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
# Install required python package
# Customed anaconda environment is not usable
# So we need to use root anaconda environment directly
pip install rosinstall rosinstall-generator wstool rosdep
# rqt tools kit
sudo apt install ros-melodic-rqt ros-melodic-rqt-common-plugins 
# add pyside2 to anaconda environment
pip install pyside2 pydot
```

## 1.3. ROS's general concepts

There's a quick read about ROS's general concepts at [ros-101-intro-to-the-robot-operating-system](https://robohub.org/ros-101-intro-to-the-robot-operating-system/). ROS is using message queuing architecture, so that's why ROS's general architecture is very similar to AMQP ((Advanced Message Queuing Protocol)) architecture (see [rabbitmq](https://www.rabbitmq.com/tutorials/amqp-concepts.html) and [medium](https://medium.com/startlovingyourself/need-of-messaging-queues-in-microservices-architecture-91de0db89120) ).
See the following 2 images for comparing.

![ROS's general concepts](/assets/images/ros/ros101-1.png)

![AMQP concept](/assets/images/ros/hello-world-example-routing.png)

There's a lot of article on the internet talking about message queuing architecture, so i won't go into the message queuing architecture detail here. 

In case of ROS, there're two types of node : **ROS Node** and **ROS Master**. **ROS Node** is process that do simple task such as *read data*, *processing data*, etc. **ROS Node** can talk to each other by sending *message* through **Topics**. Each **ROS Node** can act as *Publisher*, *Subcriber* or both. **ROS Master** is the *root* process that work as a look up table. By using this look up table, we can exchange message between each node without worrying about node's specific detail and address. 

If you're fammiliar with message queuing architecture (RabbitMQ), you'll find this similar word comparing table useful. The Publisher/Subcriber used in ROS mean that message from 1 Publisher can be used by many Subcriber. 

|  RabbitMQ  |        ROS         |
| ---------- | ------------------ |
| ROS Master | RabbitMQ           |
| ROS Node   | Publisher/Subcriber |
| Topics     | Message queue      |
| Message    | Message            |

Let's go deeper to understand how these concepts work in the following case :

```bash
1. Camera is connected to robot Arm
2. Robot read camera data and do simple image processing
3. Dev-PC (Laptop) read camera image and display it
```

![ROS's general concepts 3](/assets/images/ros/ros101-3.png)

The development process should be as follow:

```bash
1. Start the ROS Master
2. Start Camera Node, Image Processing Node, Image Display Node
3. The un-stopped loop
  a. Camera Node which read data from Camera then publish
   image data into topics called /image_data
  b. Image Processing Node and Image Display Node subcribe from /image_data topics and consume the message 
```

In the next post, i'll go into further detail about how to write ROS program and how to use image processing program with ROS.

## 1.4. Reference

1. [ros-101-intro-to-the-robot-operating-system](https://robohub.org/ros-101-intro-to-the-robot-operating-system/)
2. [ros-cheatsheet](http://www.clearpathrobotics.com/ros-cheat-sheet)

以上