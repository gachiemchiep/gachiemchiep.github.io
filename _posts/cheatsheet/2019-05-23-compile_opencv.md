---
title: "Compile OpenCV from source for Ubuntu with Cuda, Atlas"
excerpt: "Compile OpenCV 4.1.0 from source for Ubuntu 18.04 with Cuda 10.1, Atlas."
categories:
  - cheatsheet
tags:
  - opencv
published: true
comments: true
toc: true
support: true
classes: wide
order: 9
author: vugia.truong
---

# Summary

opencv-python package does not support cuda and atlas.
So in this post, i'll compile opencv 4.1.0 for Ubuntu 18.04, with Cuda, Atlas, python

## Requirement

* OS : Ubuntu 18.04
* Opencv version : 4.1.0
* Cuda : 10.1
* Python : 3.6, Anaconda

## Procedure

### Install Dependencies


```bash
# 1. Build dependencies
sudo apt install build-essential libatlas-dev liblapack-devel libtesseract-dev libopenexr-dev ffmpeg libblas-dev cmake cmake-gui

# 2. Install anaconda
# Go to https://www.anaconda.com/

# 3. Install cuda 10.1
# go to https://developer.nvidia.com/cuda-downloads 
# and do as same as the manual

```

### Download source

Download opencv's source code from [opencv's homepage](https://opencv.org/releases/) . 
And opencv contribution modules' source code from [github](https://github.com/opencv/opencv_contrib). 
I'll use the 4.1.0 version and put at the following location

```bash
~/opt
├── opencv-4.1.0
    ├── opencv-4.1.0                : source code
        ├── build                   : build directory
    ├── opencv_contrib-4.1.0        : source code of contribution modules

```

### Compiling

* Create yor custom python environment

```bash
conda create --name opencv-4.1.0_cu10 python=2
# Need numpy as python dependencies
source activate opencv-4.1.0_cu10
pip install numpy flake8
```

* Compiling

```bash
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D OPENCV_EXTRA_MODULES_PATH=~/opt/opencv-4.1.0/opencv_contrib-4.1.0/modules \
    -D OPENCV_ENABLE_NONFREE=ON \
    -D BUILD_EXAMPLES=ON ..\
    -D BUILD_NEW_PYTHON_SUPPORT=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_FORCE_PYTHON_LIBS=ON \
    -D PYTHON_DEFAULT_EXECUTABLE=$(which python) \
    -D PYTHON_EXECUTABLE=~/miniconda2/envs/opencv-4.1.0_cu10/bin/python \
    -D PYTHON_LIBRARY=~/miniconda2/envs/opencv-4.1.0_cu10/lib/libpython2.7.so \
    -D PYTHON_INCLUDE_DIRS=~/miniconda2/envs/opencv-4.1.0_cu10/include \
    -D PYTHON2_NUMPY_INCLUDE_DIRS=~/miniconda2/envs/opencv-4.1.0_cu10/lib/python2.7/site-packages/numpy \
    -D WITH_CUDA=ON \
    -D BUILD_opencv_cudacodec=OFF \
    -D CUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-10.1 \
    -D CUDA_ARCH_BIN="6.0 6.1 7.0 7.5" -D CUDA_ARCH_PTX="" \
    -D BUILD_EXAMPLES=ON ..

make -j4
```

* Or compile with both python2 and python 3 

```bash
mkdir build
cd build
# py2 and py3 are python environments corresponding for python 2.7 and python 3.7
# you can create py2 and py3 by the following commands
# conda create --name py2 python=2.7
# conda create --name py3 python=3.7
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D OPENCV_EXTRA_MODULES_PATH=~/opt/opencv/opencv_contrib-4.1.0/modules \
    -D OPENCV_ENABLE_NONFREE=ON \
    -D BUILD_EXAMPLES=ON ..\
    -D BUILD_NEW_PYTHON_SUPPORT=ON \
    -D BUILD_opencv_python3=ON \
    -D BUILD_opencv_python2=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_FORCE_PYTHON_LIBS=ON \
    -D PYTHON2_EXECUTABLE=~/miniconda3/envs/py2/bin/python \
    -D PYTHON2_LIBRARY=~/miniconda3/envs/py2/lib/libpython2.7.so \
    -D PYTHON2_INCLUDE_DIRS=~/miniconda3/envs/py2/include \
    -D PYTHON2_NUMPY_INCLUDE_DIRS=~/miniconda3/envs/py2/lib/python2.7/site-packages/numpy \
    -D PYTHON3_EXECUTABLE=~/miniconda3/envs/py3/bin/python \
    -D PYTHON3_LIBRARY=~/miniconda3/envs/py3/lib/libpython3.7m.so \
    -D PYTHON3_INCLUDE_DIRS=~/miniconda3/envs/py3/include \
    -D PYTHON3_NUMPY_INCLUDE_DIRS=~/miniconda3/envs/py3/lib/python3.7/site-packages/numpy \
    -D WITH_CUDA=ON \
    -D WITH_FFMPEG=1 \
    -D WITH_TIFF=ON \
    -D CUDA_FAST_MATH=1 \
    -D WITH_CUBLAS=1 \
    -D BUILD_opencv_cudacodec=OFF \
    -D CUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-10.1 \
    -D CUDA_ARCH_BIN="6.0 6.1 7.0 7.5" -D CUDA_ARCH_PTX="" \
    -D BUILD_EXAMPLES=ON ..

make -j4
# The output python 2
# build/lib/cv2.so
# The output python 3
# build/lib/python3/cv2.cpython-37m-x86_64-linux-gnu.so
```

* How to change build's options

```bash
# Use the following command to list all of CMake build options
cmake -LA | awk '{if(f)print} /-- Cache values/{f=1}'
# output example
WITH_CUDA:BOOL=ON
```

If you find that the above command line is hard to read, please use cmake-gui . It is the most convinient method. 

* Link python into current miniconda environment

```bash
# Python wrapper will be built in the following location
lib
└── cv2.so
# in some case it maybe as follow
# Please check your environment
lib/python3/
└── cv2.cpython-37m-x86_64-linux-gnu.so

# Link into our anaconda environment
cd /home/gachiemchiep/miniconda2/envs/opencv-4.1.0_cu10/lib/python2.7/site-packages
 ln -s ~/opt/opencv-4.1.0/opencv-4.1.0/build/lib/cv2.so ${PWD}/cv2.so
```


* Checking

```bash
# Start python console
python
# Inside python console
import cv2
print(cv2.getBuildInformation())
```

BuildInformation is as follow

```bash
(base) gachiemchiep@Home:~/miniconda2/envs/opencv-4.1.0_cu10/lib/python2.7/site-packages$ python
Python 2.7.15 |Anaconda, Inc.| (default, Dec 14 2018, 19:04:19) 
[GCC 7.3.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
>>> print(cv2.getBuildInformation())

General configuration for OpenCV 4.1.0 =====================================
  Version control:               unknown

  Extra modules:
    Location (extra):            /home/gachiemchiep/opt/opencv-4.1.0/opencv_contrib-4.1.0/modules
    Version control (extra):     unknown

  Platform:
    Timestamp:                   2019-05-23T13:10:13Z
    Host:                        Linux 4.15.0-50-generic x86_64
    CMake:                       3.10.2
    CMake generator:             Unix Makefiles
    CMake build tool:            /usr/bin/make
    Configuration:               RELEASE

  CPU/HW features:
    Baseline:                    SSE SSE2 SSE3
      requested:                 SSE3
    Dispatched code generation:  SSE4_1 SSE4_2 FP16 AVX AVX2 AVX512_SKX
      requested:                 SSE4_1 SSE4_2 AVX FP16 AVX2 AVX512_SKX
      SSE4_1 (15 files):         + SSSE3 SSE4_1
      SSE4_2 (2 files):          + SSSE3 SSE4_1 POPCNT SSE4_2
      FP16 (1 files):            + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 AVX
      AVX (5 files):             + SSSE3 SSE4_1 POPCNT SSE4_2 AVX
      AVX2 (29 files):           + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 FMA3 AVX AVX2
      AVX512_SKX (2 files):      + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 FMA3 AVX AVX2 AVX_512F AVX512_SKX

  C/C++:
    Built as dynamic libs?:      YES
    C++ Compiler:                /usr/bin/c++  (ver 7.4.0)
    C++ flags (Release):         -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wuninitialized -Winit-self -Wno-delete-non-virtual-dtor -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -O3 -DNDEBUG  -DNDEBUG
    C++ flags (Debug):           -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wuninitialized -Winit-self -Wno-delete-non-virtual-dtor -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -g  -O0 -DDEBUG -D_DEBUG
    C Compiler:                  /usr/bin/cc
    C flags (Release):           -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wuninitialized -Winit-self -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -O3 -DNDEBUG  -DNDEBUG
    C flags (Debug):             -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wuninitialized -Winit-self -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -g  -O0 -DDEBUG -D_DEBUG
    Linker flags (Release):      -Wl,--gc-sections  
    Linker flags (Debug):        -Wl,--gc-sections  
    ccache:                      NO
    Precompiled headers:         YES
    Extra dependencies:          m pthread cudart_static -lpthread dl rt nppc nppial nppicc nppicom nppidei nppif nppig nppim nppist nppisu nppitc npps cublas cufft -L/usr/local/cuda-10.1/lib64 -L/usr/lib/x86_64-linux-gnu -L/usr/local/cuda/lib64
    3rdparty dependencies:

  OpenCV modules:
    To be built:                 aruco bgsegm bioinspired calib3d ccalib core cudaarithm cudabgsegm cudafeatures2d cudafilters cudaimgproc cudalegacy cudaobjdetect cudaoptflow cudastereo cudawarping cudev datasets dnn dnn_objdetect dpm face features2d flann freetype fuzzy gapi hdf hfs highgui img_hash imgcodecs imgproc line_descriptor ml objdetect optflow phase_unwrapping photo plot python2 quality reg rgbd saliency sfm shape stereo stitching structured_light superres surface_matching text tracking ts video videoio videostab viz xfeatures2d ximgproc xobjdetect xphoto
    Disabled:                    cudacodec world
    Disabled by dependency:      -
    Unavailable:                 cnn_3dobj cvv java js matlab ovis python3
    Applications:                tests perf_tests examples apps
    Documentation:               NO
    Non-free algorithms:         YES

  GUI: 
    GTK+:                        YES (ver 2.24.32)
      GThread :                  YES (ver 2.56.4)
      GtkGlExt:                  NO
    VTK support:                 YES (ver 6.3.0)

  Media I/O: 
    ZLib:                        /usr/lib/x86_64-linux-gnu/libz.so (ver 1.2.11)
    JPEG:                        /usr/lib/x86_64-linux-gnu/libjpeg.so (ver 80)
    WEBP:                        /usr/lib/x86_64-linux-gnu/libwebp.so (ver encoder: 0x020e)
    PNG:                         /usr/lib/x86_64-linux-gnu/libpng.so (ver 1.6.34)
    TIFF:                        /usr/lib/x86_64-linux-gnu/libtiff.so (ver 42 / 4.0.9)
    JPEG 2000:                   build (ver 1.900.1)
    OpenEXR:                     /usr/lib/x86_64-linux-gnu/libImath.so /usr/lib/x86_64-linux-gnu/libIlmImf.so /usr/lib/x86_64-linux-gnu/libIex.so /usr/lib/x86_64-linux-gnu/libHalf.so /usr/lib/x86_64-linux-gnu/libIlmThread.so (ver 2.2.0)
    HDR:                         YES
    SUNRASTER:                   YES
    PXM:                         YES
    PFM:                         YES

  Video I/O:
    DC1394:                      YES (2.2.5)
    FFMPEG:                      YES
      avcodec:                   YES (57.107.100)
      avformat:                  YES (57.83.100)
      avutil:                    YES (55.78.100)
      swscale:                   YES (4.8.100)
      avresample:                YES (3.7.0)
    GStreamer:                   NO
    v4l/v4l2:                    YES (linux/videodev2.h)

  Parallel framework:            pthreads

  Trace:                         YES (with Intel ITT)

  Other third-party libraries:
    Intel IPP:                   2019.0.0 Gold [2019.0.0]
           at:                   /home/gachiemchiep/opt/opencv-4.1.0/opencv-4.1.0/build/3rdparty/ippicv/ippicv_lnx/icv
    Intel IPP IW:                sources (2019.0.0)
              at:                /home/gachiemchiep/opt/opencv-4.1.0/opencv-4.1.0/build/3rdparty/ippicv/ippicv_lnx/iw
    Lapack:                      YES (/usr/lib/x86_64-linux-gnu/liblapack.so /usr/lib/x86_64-linux-gnu/libcblas.so /usr/lib/x86_64-linux-gnu/libatlas.so)
    Eigen:                       YES (ver 3.3.4)
    Custom HAL:                  NO
    Protobuf:                    build (3.5.1)

  NVIDIA CUDA:                   YES (ver 10.1, CUFFT CUBLAS NVCUVID)
    NVIDIA GPU arch:             60 61 70 75
    NVIDIA PTX archs:

  OpenCL:                        YES (no extra features)
    Include path:                /home/gachiemchiep/opt/opencv-4.1.0/opencv-4.1.0/3rdparty/include/opencl/1.2
    Link libraries:              Dynamic load

  Python 2:
    Interpreter:                 (ver 2.7.15)
    Libraries:                   /usr/lib/x86_64-linux-gnu/libpython2.7.so (ver 2.7.15rc1)
    numpy:                       /home/gachiemchiep/miniconda2/envs/opencv-4.1.0_cu10/lib/python2.7/site-packages/numpy (ver 1.13.3)
    install path:                /cv2/python-2.7

  Python (for build):            /home/gachiemchiep/miniconda2/envs/opencv-4.1.0_cu10/bin/python

  Java:                          
    ant:                         NO
    JNI:                         /usr/lib/jvm/java-8-oracle/include /usr/lib/jvm/java-8-oracle/include/linux /usr/lib/jvm/java-8-oracle/include
    Java wrappers:               NO
    Java tests:                  NO

  Install to:                    /usr/local
-----------------------------------------------------------------


```

## List of bugs

There are some bugs that is occured during the compiling

* fatal error: nvcuvid.h: No such file or directory

```bash
# Bug detail
In file included from /home/gachiemchiep/opt/opencv-4.1.0/opencv-4.1.0/build/modules/cudacodec/opencv_cudacodec_pch_dephelp.cxx:1:0:
/home/gachiemchiep/opt/opencv-4.1.0/opencv_contrib-4.1.0/modules/cudacodec/src/precomp.hpp:62:18: fatal error: nvcuvid.h: No such file or directory
         #include <nvcuvid.h>
                  ^~~~~~~~~~~
# Solution : disable cudacodec compiling
-D BUILD_opencv_cudacodec=OFF 
# Detail: https://github.com/opencv/opencv_contrib/issues/1786
```

* messing between python2 and python3 

```bash
# cmake keep using python2 inside /usr/bin/python as compiler instead of anaconda python

# Solution : use the following options
-D PYTHON_DEFAULT_EXECUTABLE=$(which python)
# Detail : https://stackoverflow.com/questions/37070304/how-to-build-opencv-for-python3-when-both-python2-and-python3-are-installed
```

End
