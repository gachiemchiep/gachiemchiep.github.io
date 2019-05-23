---
title: "Learning ROS. Part 3"
excerpt: "Some quick notes when learning ROS."
categories: 
  - tutorial
tags: 
  - ros
comments: true
published: true
toc: true
support: true
order: 9
author: vugia.truong
---

# Using ROS launch file

As in the part 2 of this learning process, i created simple video streaming using ROS. 
But openning terminal for each node is very time-consuming and unconvinient.
ROS provides a mechanism to start master node and other nodes at once, which call [ros launch file](http://wiki.ros.org/roslaunch). In this post, i'll go through process of creating simple launch file for the last tut3

Source code is at [github](https://github.com/gachiemchiep/ROS-tutorial). 

## Create workspace

Our workspace is as follow.

```bash
# we need to copy tut3 directory into tut4
# and edit settings of CMakeLists.txt and package.xml
ROS-tutorial/src/tut4
    ├── include
    ├── launch
    │   └── run_tut4.launch         <-- 1. launch file
    └── src                         <-- 2. source code, no-change
        ├── Camera_Node.cpp
        ├── Img_Display_Node.cpp
        └── Img_Process_Node.cpp
    ├── CMakeLists.txt
    ├── package.xml

```

Remember to change the following lines after copying or the build will failed.

```bash
# CMakeLists.txt
project(tut4)
add_executable(tut4_Camera_Node src/Camera_Node.cpp)
target_link_libraries(tut4_Camera_Node ${catkin_LIBRARIES}  ${OpenCV_LIBS})
add_executable(tut4_Img_Process_Node src/Img_Process_Node.cpp)
target_link_libraries(tut4_Img_Process_Node ${catkin_LIBRARIES}  ${OpenCV_LIBS})
add_executable(tut4_Img_Display_Node src/Img_Display_Node.cpp)
target_link_libraries(tut4_Img_Display_Node ${catkin_LIBRARIES}  ${OpenCV_LIBS})

# package.xml
<name>tut4</name>
  <version>0.0.0</version>
  <description>The tut4 package</description>
```

## Grammar of launch file

Launch file uses xml syntax. List a syntax and meaning is as follow. See [roslaunch xml](http://wiki.ros.org/roslaunch/XML) for more detail

```xml
<launch>
  <!-- we can include file from other package -->
  <include file="$(find package)/xxx.launch"/>
  <!-- launch file arguments : ??? -->
  <arg 
    name="test1"
    default="0"
  />
  <!-- group = collection of node; have ns=namespace -->
  <group ns="group_name" if="$(arg test1)">
    <param name="huey" value="red" comment="parameter of group"/>
    <node 
      pkg="package_name"
      type="executable_name"
      name="node_name"  comment="node will use this name, override ros::init"
      output="screen"   comment="direct output to terminal, not useful"
      respawn="true"    comment="respawn process when fail"
      required="true"   comment="this node is must have"
                        comment="respawn+required can not both set to true"
      launch-prefix="xterm -e"  comment="launch command prefix, use xterm -e to open separated windows, very useufl"
    >
      <!-- rename the topics that node publish/subcribe to -->
      <remap from="original_name" to="new_name"/>
      <param name="huey" value="red" comment="parameter of node"/>
    </node>

  <group/>
</launch>
```

## Write simple launch file

Our launch file is as follow. Remember that launch file have to be put inside *launch* directory or ROS will not run correctly.

```xml
<launch>
	<group ns="tut4">
		<node 
			pkg="tut4"
			type="tut4_Camera_Node"
			name="Camera_Node"
			launch-prefix="xterm -e"		
		>
		</node>
		<node 
			pkg="tut4"
			type="tut4_Img_Display_Node"
			name="Img_Display_Node"
			launch-prefix="xterm -e"		
		>
		</node>
		<node 
			pkg="tut4"
			type="tut4_Img_Process_Node"
			name="Img_Process_Node"
			launch-prefix="xterm -e"		
		>
		</node>
		
	</group>
</launch>
```

**Note:** the launch file above will open new terminal window for each node. If we want to open only 1 new window, and add node's console into that window's tab, we should use [tmux](https://github.com/tmuxinator/tmuxinator). This is out of this post's scope.

## Run launch file

ROS use the *roslaunch* command to execute launch file. Detail is at [roslaunch](http://wiki.ros.org/roslaunch). In our case the command is as follow.

```bash
# Remember to rebuild the package
catkin_make
source devel/setup.bash
# roslaunch package-name launch-file-name.launch
# add -v to roslaunch for verbosing
roslaunch tut4 run_tut4.launch
```

After run launch file, the console/terminal output is as follow.

```bash
(base) gachiemchiep@Home:~/workspace/github/ROS-tutorial$ roslaunch tut4 run_tut4.launch
... logging to /home/gachiemchiep/.ros/log/1a490b32-6faa-11e9-8a4d-207918e2c14c/roslaunch-Home-24486.log
Checking log directory for disk usage. This may take awhile.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://Home:43529/

SUMMARY
========

PARAMETERS
 * /rosdistro: melodic
 * /rosversion: 1.14.3

NODES
  /tut4/
    Camera_Node (tut4/tut4_Camera_Node)
    Img_Display_Node (tut4/tut4_Img_Display_Node)
    Img_Process_Node (tut4/tut4_Img_Process_Node)

auto-starting new master
process[master]: started with pid [24496]
ROS_MASTER_URI=http://localhost:11311

setting /run_id to 1a490b32-6faa-11e9-8a4d-207918e2c14c
process[rosout-1]: started with pid [24507]
started core service [/rosout]
process[tut4/Camera_Node-2]: started with pid [24510]
process[tut4/Img_Display_Node-3]: started with pid [24514]
process[tut4/Img_Process_Node-4]: started with pid [24523]
```

Inside our launch file, we use the *launch-prefix="xterm -e"* to open new terminal for each node. So after executing, 3 new terminal will be opened as follow picture.

![pt3-execution](/assets/images/ros/ros-tut4-img.png)

The *rqt_graph* is as follow. Please notice the *tut4* rectangle. This mean 3 nodes *Camera_Node* , *Img_Display_Node*, *Img_Process_Node* and *img_data* topic are inside *tut4* namespace. When display node's name, rqt_graph will append the *tut4* in front of each node and topics name. 

![pt3-graph](/assets/images/ros/ros-tut4-graph.png)

We use the *Ctrl+C* shortcut to shutdown execution of launch file. 

```bash
^C
[tut4/Img_Process_Node-4] killing on exit
[tut4/Img_Display_Node-3] killing on exit
[tut4/Camera_Node-2] killing on exit
[rosout-1] killing on exit
[master] killing on exit
shutting down processing monitor...
... shutting down processing monitor complete
done

```



## Reference

1. [roslaunch xml](http://wiki.ros.org/roslaunch/XML)
2. [roslaunc blog (in japanese)](https://kazuyamashi.github.io/ros_lecture/ros_launch.html)
3. [ros launch big project](http://wiki.ros.org/action/fullsearch/roslaunch/Tutorials/Roslaunch%20tips%20for%20larger%20projects?action=fullsearch&context=180&value=linkto%3A%22roslaunch%2FTutorials%2FRoslaunch+tips+for+larger+projects%22)


以上