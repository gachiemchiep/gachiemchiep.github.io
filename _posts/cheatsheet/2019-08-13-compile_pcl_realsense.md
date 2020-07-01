---
title: "Compile PCL + OpenCV + librealsense2"
excerpt: "Compile PCL + OpenCV + librealsense2 from source."
categories:
  - cheatsheet
tags:
  - opencv
published: true
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---

# Summary

So in this post, i'll compile PCL + OpenCV + librealsense2 from source.

# Requirement

* OS : Ubuntu 18.04
* Opencv version : 4.1.0
* Cuda : 10.1
* Python : 3.6, Anaconda
* Realsense D435
* librealsense : 2.24
* PCL : 1.9

# Procedure

## OpenCV 4.1.0 with cuda

Please see [Compile OpenCV from source for Ubuntu with Cuda, Atlas](/vision/compile_opencv/)


## PCL 1.9

* clone the opencv-4.1.0_cu10 cu10 into *pcl* environment

```bash
conda create --name pcl --clone opencv-4.1.0_cu10
conda activate pcl
```

* [compile pcl from source](http://pointclouds.org/documentation/tutorials/compiling_pcl_posix.php)

```bash
# At August 2019, i can not access the below respo
# ppa:v-launchpad-jochen-sprickerhof-de/pcl
# so i need to compile pcl from source


## pcl dependencies
### others
sudo apt -y install libflann libboost-all-dev libeigen3-dev
### vtk 8.2.0
wget https://www.vtk.org/files/release/8.2/VTK-8.2.0.tar.gz
tar xfv VTK-8.2.0.tar.gz 
cd VTK-8.2.0/
mkdir build
cd build/
cmake ..
make -j5
sudo make install


### pcl
git clone https://github.com/PointCloudLibrary/pcl
cd pcl/
mkdir build
cd build/
cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo ..
cmake ..
sudo make install

```

* test pcl 

Use the [pcl_visualizer](http://www.pointclouds.org/documentation/tutorials/pcl_visualizer.php) as test code. CMakeLists.txt and source code both are available. 
If we want to use pcl with Eclipse, we should follow [Using PCL with Eclipse](http://www.pointclouds.org/documentation/tutorials/using_pcl_with_eclipse.php#using-pcl-with-eclipse) . There's other example at [turorials](http://www.pointclouds.org/documentation/tutorials/)


##  librealsense2 + python wrapper + pcl 


```bash
# activate python env
conda activate pcl
# librealsense 2 + python wrapper
cmake ../ -DBUILD_PYTHON_BINDINGS:bool=true -DPYTHON_EXECUTABLE=$(which python) -DBUILD_PCL_EXAMPLES=true
make -j4
sudo make install

# copy librealsense2 to python environment
cp wrappers/python/pyrealsense2.cpython-37m-x86_64-linux-gnu.so /home/jil/opt/miniconda2/envs/pcl/lib/python3.7/site-packages/pyrealsense2.so 

```

* test librealsense2 + python wrapper 
  
  
i use the demo script at: [python-tutorial-1-depth.py](https://github.com/IntelRealSense/librealsense/blob/master/wrappers/python/examples/python-tutorial-1-depth.py) . The depth's raw output will be printout in the console. For example :

```bash
       WWWWWWX:     :WWWWWWWhn.                            XW   
      hWBn.  nXWBn. nXXWWWWWWWWWXhn.                       XW.  
      n.       hXXXBB. .hWWWWWWWWWWWWBh:                   BW   
                 ::nh.   nXWWWWWWWWWWWWWWBhn.              BB.  
       .             .:  BWWWWWWWWWWWWWWWWWWWWBh:               
                   .nn.nnXWXWWWWWWWWWWWWWWWWWWWWWWX:            
                 ..nn    XWXhWWWWWWWWWWWWWWWWWWWWWWWWBnnn.      
                 nhh.    XWWWWWWWWXWXWWWWWWWWWWWWWWWWWWWXB:     
               n:hhnh.   :hWWWWWWWXWWWWXXhWWWWWWWWWWWWWWWXBh:nnX
                 :Bn     hBhXXWWWWWWWWWWWBhXWWWXXBhBX:hBXWWn .n:
                         hn.BBWWWWWWWWWWWWWWWWWWWWXh:.    :nn.::
                    .    nWhhBWWWWWXXWWWWWWWWWWWWXnhnhXXn:     h
                    :.   hWBWBXWWWWWWWWWWWWWWWWWWWWWWWWWWWWXh:. 
                         :B   hhnBnhBXWWWWWWWWWWhWWWWXBBhBXBhh:B
                Xh       ..    nnn:nBWXBWWWWWWWWBWBWWWWX:n:  . :
                 .  h.         .  Bh . ..hWWWWWW.BWWWWXXnBXWn: :
                    Wn   .:   ::hn:::n:hWWWWWWWWBWWWWWWWhWWWBhhW
                    :.   n.    :hh:::hXXWWWWWWWWWWWWWWWWWWWWWWWW
                       ::XWX.  .nnnn  .XXBXXWWWXWWWWWWWWWWWWWWWW
                         nWWBh.     .n:h.nWWWWWWXWWWWWWWWWWWWWWW
                           .nn.          hhXWXWWXWWXXBWWWWWWWWWW
                               ...            :hnnnhWWWBBXWWWWWW
                          nn: nhXWWXWXXXXWWWWWWWWWWWXhBXWWXWXXXX
                                  .  .nBXWWWXWWWWWWWWWWh:BWWWXnW
      :WWWWWWWWWWWWWWWWWWWWWXn.                            XX   
      .WWWWWWWWWWWWWWWWWWWWWWWWWXn..                       WX.  
      nWWWWWWWWWWWWWWWWWWWWWWWWWWWWWWXh:                   WW.  
      nWWWWWWWWWWWWWWWWWWWWWWWWWWWWWWWWWWXhn.              BB   
.....

```

There're other examples at [librealsense python example](https://github.com/IntelRealSense/librealsense/tree/master/wrappers/python/examples)


* test librealsense2 + pcl

Run the following command

```bash
rs-pcl
```

There're also several example at [librealsen pcl](https://github.com/IntelRealSense/librealsense/tree/master/wrappers/pcl). 


## Reference

1. [compile pcl ubuntu 16.04](https://askubuntu.com/questions/916260/how-to-install-point-cloud-library-v1-8-pcl-1-8-0-on-ubuntu-16-04-2-lts-for)
2. [compile pcl from source](http://pointclouds.org/documentation/tutorials/compiling_pcl_posix.php)
3. [pcl turorials](http://www.pointclouds.org/documentation/tutorials/)
4. [librealsense python wrapper ](https://github.com/IntelRealSense/librealsense/tree/master/wrappers/python/examples)
5. [librealsense pcl wrapper ](https://github.com/IntelRealSense/librealsense/tree/master/wrappers/pcl)

End
