---
title: "Learning ROS. Part 2"
excerpt: "Some quick notes when learning ROS."
categories: 
  - ROS
tags: 
  - tutorial
comments: true
published: false
toc: true
support: true
order: 9
author: vugia.truong
---
# ROS note

## Log Message

[ROS log level](http://wiki.ros.org/Verbosity%20Levels)

```bash
# 5 levels
DEBUG
INFO    <---- Default level
WARN
ERROR
FATAL
```

Log locations : 

```bash
# 1. Console:
# DEBUG + INFO = stdout, WARN + ERROR + FATAL = stderr
# 2. /rosout
# we could use the rqt_console to read all log from /rosout
# to test, use the rostopic echo /rosout
# 3. log files
# location : ∼/.ros/log/run_id/rosout.log
# When we run roscore, we will abtain the run_id as follow
#...
# setting /run_id to b2eb73dc-6f2f-11e9-8a4d-207918e2c14c
# ...
# Check log's size
# rosclean check 
# clean current log
# rosclean purge
```

Change log level

```bash
# command line <- not very useful
rosservice call /node-name/set_logger_level ros.package-name level
# GUI <- very useful
rqt_logger_level
# C++ code <- not very useful
```

## parameters

ROS also provides mechanism to change/update node's setting at runtime. 
How : ROS use *parameter server* which is a shared, multi-variate dictionary that is 
accessible via network APIs. *parameter server* can store + retrieve parameter at runtime

-> personally, this is pretty cool

```bash
# Use the rosparam utility to access/change setting value on parameter server
# List parameter server
rosparam list
# Get value of parameters
rosparam get parameter_namespace
# namepsace = / -> get all parameters
# Set value of parameter
rosparam set parameter_namespace parameter_value
# Dump parameters into file
rosparam dump filename parameter_namespace
# Load parameters from file into namespace
rosparam load filename parameter_namespace

# Note : ROS parameter use the yaml format
# See https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html
# for more detail about yaml
# So the dump, and load should add the .yaml extension

```



1. [ros parameter server cpp](http://wiki.ros.org/roscpp/Overview/Parameter%20Server)
2. [ros parameter server](http://wiki.ros.org/Parameter%20Server)
3. [rosparam](http://wiki.ros.org/rosparam)

## ROS service

Sometimes we need RPC : request/reply model. 
ROS has *ROS service* which could be use to implement RPC.
Currently we don't need this *ROS service* feature

## Recording and replaying message

-> can use to : looping robot

```bash
rosbag record -O filename.bag topic-names
rosbag play filename.bag
rosbag info filename.bag

rosrun turtlesim draw_square
rosbag record -O square.bag /turtle1/cmd_vel /turtle1/pose
rosbag play square.bag

```

bag can be used in launch file to

```xml
<node
  pkg="rosbag"
  name="record"
  type="record"
  args="-O filename.bag topic-names"
/>

<node
pkg="rosbag"
name="play"
type="play"
args="filename.bag"
/>
```

## Reference


以上