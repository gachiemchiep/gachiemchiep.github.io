
## Ros official tutorial

-> Tutorial is at : [tutorial_link](http://wiki.ros.org/ROS/Tutorials) 
 -> very hard to follow and understand

### Installing and Configuring ROS environment

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
sudo apt update
sudo apt install ros-melodic-desktop-full
sudo rosdep init
rosdep update
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
source /opt/ros/melodic/setup.bash
pip install rosinstall rosinstall-generator wstool rosdep
sudo apt install build-essential
# rqt tools kit
sudo apt install ros-melodic-rqt ros-melodic-rqt-common-plugins 
# add pyside2 to anaconda environment
pip install pyside2 pydot
```

### Create ROS workspace

```bash
 2017  mkdir -p catkin_ws/src
 2018  cd catkin_ws/
 2019  catkin_make
 2020  source devel/setup.bash 
 2021  echo $ROS_PACKAGE_PATH

```

### Navigating the ROS file system

```bash
# pack, cd, ls command of ros filesystem
# totally separated from linux filesystem
rospack
roscd
rosls
```

### Creating a ROS package

```bash
catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
cd ..
catkin_make
source devel/setup.bash 
rospack depends beginner_tutorials 
```

### Building ROS package

```bash
catkin_make
catkin_make install

```

### Understanding ROS Nodes

```bash
# ROS use the pipeline approach

Nodes: A node is an executable that uses ROS to communicate with other nodes.
Messages: ROS data type used when subscribing or publishing to a topic.
Topics: Nodes can publish messages to a topic as well as subscribe to a topic to receive messages.
Master: Name service for ROS (i.e. helps nodes find each other)
rosout: ROS equivalent of stdout/stderr
roscore: Master + rosout + parameter server (parameter server will be introduced later)

# ROS client libraries allow nodes written in different programming languages to communicate with each other
rospy = python client library
roscpp = c++ client library

```

How to run ROS program

```bash
# Load the environment
source /opt/ros/melodic/setup.bash
# run the roscore
# only one process is allowed at run time
roscore
# Open other terminate
# Then run other program
# rosrun [package_name] [node_name] __name:=node_name
rosrun turtlesim turtlesim_node
```

```bash
roscore = ros+core : master (provides name service for ROS) + rosout (stdout/stderr) + parameter server (parameter server will be introduced later)
rosnode = ros+node : ROS tool to get information about a node.
rosrun = ros+run : runs a node from a given package.
```

### Understanding ROS topic

```bash
# command : rostopic, rqt_plot

# start the demo
roscore
# run the turtlesim
rosrun turtlesim turtlesim_node __name:=turtle1
# run the turtlesim's keyboard
rosrun turtlesim turtle_teleop_key
# Draw the called-graph
# This graph is updated in real-time
# so we don't need to close-open everytime
rosrun rqt_graph rqt_graph
# Use rostopic command to get information about topic
#
rostopic echo [topic_name]
rostopic type [topic_name]
# show the detail of data type
rosmsg show [data_type]
# We can use the pub command to push message into topic
# -> good for debug
rostopic pub [topic_name] [msg_type] [args]
# example 
$ rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'

# rostopic hz reports the rate at which data is published.

# rqt_plot
-> rqt_plot displays a scrolling time plot of the data published on topics. 

```

### Understanding ROS services and Parameters

```bash
Why we need ROS services
    -> do not know
How we use ROS services
    -> do not know
What is ROS services
    -> similar to systemctl command of linux
    -> can be used to restart, stop, etc

# service related command
rosservice list         print information about active services
rosservice call         call the service with the provided args
rosservice type         print service type
rosservice find         find services by service type
rosservice uri          print service ROSRPC uri

rosparam
    -> store and manipulate data on the ROS parameter server
    -> parameter server = configuration

rosparam list
rosparam set [param_name]
rosparam get [param_name]
rosparam  dump [filename] [namespace]
rosparam load [filename] [namespace]
```

### 