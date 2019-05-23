---
title: "Compile OpenCV from source for Ubuntu with Cuda, Atlas"
excerpt: "Compile OpenCV from source for Ubuntu 18.04 with Cuda 10.1, Atlas."
categories:
  - vision
tags:
  - tutorial
  - opencv
published: false
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---

# Summary

opencv-python package does not support cuda and atlas.
So in this post, i'll compile opencv 3.5.6 for Ubuntu 18.04, with Cuda, Atlas, python

## Requirement

* OS : Ubuntu 18.04
* Opencv version : 3.5.6
* Cuda : 10.1
* Python : 3.6, Anaconda

## Procedure

### Dependencies


```bash
# 1. Build dependencies
sudo apt install build-essential libatlas-dev liblapack-devel libtesseract-dev libopenexr-dev ffmpeg libblas-dev make-gui

# 2. Install anaconda
# Go to https://www.anaconda.com/

# 3. Install cuda 10.1
# go to https://developer.nvidia.com/cuda-downloads 
# and do as same as the manual

# 4. Create yor custom python environment
conda create --name opencv-3.4.5_cu10 python==2.7
# Need numpy as python dependencies
source activate opencv-3.4.5_cu10
pip install numpy
```

### Download source

Download opencv's source code from [opencv's homepage](https://opencv.org/releases/) . 
And opencv contribution modules' source code from [github](https://github.com/opencv/opencv_contrib). 
I'll use the 3.4.5 version and put at the following location

```bash
~/opt
├── opencv-3.4.5
    ├── opencv-3.4.5                : source code
        ├── build                   : build directory
    ├── opencv_contrib-3.4.5        : source code of contribution modules

```

### Compiling

Opencv use CMake as default build tools. Personally, changing CMake options is very nuissance
so for convinient, i used **cmake-gui** tool. 
To re-procedure the result, you need to download the CMakeCache.txt from [link](/assets/images/opencv/CmakeCache.txt) . 
Then put **CMakeCache.txt** inside **build** directory and **run cmake-gui** -> **configure** -> **generate** .

TODO : add image later

* Build

```bash
make -j6
```

* Linking python

```bash
# Python wrapper will be built in the following location
lib/python3/
└── cv2.cpython-37m-x86_64-linux-gnu.so
# Link into our anaconda environment
cd /home/gachiemchiep/opt/miniconda2/envs/opencv_3.4.5_cu10/lib/python3.7/site-packages
 ln -s ~/opt/opencv-3.4.5/opencv-3.4.5/build/lib/python3/cv2.cpython-37m-x86_64-linux-gnu.so ${PWD}/cv2.so
# If you use different environment name or python 2.7
# Please change the above line
```

* Checking

```bash
# Start python console
python
# Inside python console
import cv2
print(cv2.getBuildInformation())
```

* Output is as follow

```bash
General configuration for OpenCV 3.4.5 =====================================
  Version control:               unknown

  Extra modules:
    Location (extra):            /home/gachiemchiep/opt/opencv-3.4.5/opencv_contrib-3.4.5/modules
    Version control (extra):     unknown

  Platform:
    Timestamp:                   2019-05-22T08:06:12Z
    Host:                        Linux 4.15.0-48-generic x86_64
    CMake:                       3.10.2
    CMake generator:             Unix Makefiles
    CMake build tool:            /usr/bin/make
    Configuration:               Release

  CPU/HW features:
    Baseline:                    SSE SSE2 SSE3
      requested:                 SSE3
    Dispatched code generation:  SSE4_1 SSE4_2 FP16 AVX AVX2 AVX512_SKX
      requested:                 SSE4_1 SSE4_2 AVX FP16 AVX2 AVX512_SKX
      SSE4_1 (6 files):          + SSSE3 SSE4_1
      SSE4_2 (2 files):          + SSSE3 SSE4_1 POPCNT SSE4_2
      FP16 (1 files):            + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 AVX
      AVX (6 files):             + SSSE3 SSE4_1 POPCNT SSE4_2 AVX
      AVX2 (12 files):           + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 FMA3 AVX AVX2
      AVX512_SKX (1 files):      + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 FMA3 AVX AVX2 AVX_512F AVX512_SKX

  C/C++:
    Built as dynamic libs?:      YES
    C++11:                       YES
    C++ Compiler:                /usr/bin/c++  (ver 7.4.0)
    C++ flags (Release):         -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wuninitialized -Winit-self -Wno-narrowing -Wno-delete-non-virtual-dtor -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -O3 -DNDEBUG  -DNDEBUG
    C++ flags (Debug):           -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wuninitialized -Winit-self -Wno-narrowing -Wno-delete-non-virtual-dtor -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -g  -O0 -DDEBUG -D_DEBUG
    C Compiler:                  /usr/bin/cc
    C flags (Release):           -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wuninitialized -Winit-self -Wno-narrowing -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -O3 -DNDEBUG  -DNDEBUG
    C flags (Debug):             -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wuninitialized -Winit-self -Wno-narrowing -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -g  -O0 -DDEBUG -D_DEBUG
    Linker flags (Release):      
    Linker flags (Debug):        
    ccache:                      NO
    Precompiled headers:         YES
    Extra dependencies:          m pthread cudart_static -lpthread dl rt nppc nppial nppicc nppicom nppidei nppif nppig nppim nppist nppisu nppitc npps cublas cufft -L/usr/local/cuda-10.1/lib64 -L/usr/lib/x86_64-linux-gnu
    3rdparty dependencies:

  OpenCV modules:
    To be built:                 aruco bgsegm bioinspired calib3d ccalib core cudaarithm cudabgsegm cudafeatures2d cudafilters cudaimgproc cudalegacy cudaobjdetect cudaoptflow cudastereo cudawarping cudev datasets dnn dnn_objdetect dpm face features2d flann freetype fuzzy hdf hfs highgui img_hash imgcodecs imgproc java_bindings_generator line_descriptor ml objdetect optflow phase_unwrapping photo plot python3 python_bindings_generator reg rgbd saliency shape stereo stitching structured_light superres surface_matching text tracking ts video videoio videostab viz xfeatures2d ximgproc xobjdetect xphoto
    Disabled:                    cudacodec world
    Disabled by dependency:      -
    Unavailable:                 cnn_3dobj cvv java js matlab ovis python2 sfm
    Applications:                tests perf_tests apps
    Documentation:               NO
    Non-free algorithms:         NO

  GUI: 
    GTK+:                        YES (ver 3.22.30)
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

  Video I/O:
    DC1394:                      YES (ver 2.2.5)
    FFMPEG:                      YES
      avcodec:                   YES (ver 57.107.100)
      avformat:                  YES (ver 57.83.100)
      avutil:                    YES (ver 55.78.100)
      swscale:                   YES (ver 4.8.100)
      avresample:                YES (ver 3.7.0)
    GStreamer:                   
      base:                      YES (ver 1.14.1)
      video:                     YES (ver 1.14.1)
      app:                       YES (ver 1.14.1)
      riff:                      YES (ver 1.14.1)
      pbutils:                   YES (ver 1.14.1)
    libv4l/libv4l2:              NO
    v4l/v4l2:                    linux/videodev2.h

  Parallel framework:            pthreads

  Trace:                         YES (with Intel ITT)

  Other third-party libraries:
    Intel IPP:                   2019.0.0 Gold [2019.0.0]
           at:                   /home/gachiemchiep/opt/opencv-3.4.5/opencv-3.4.5/build/3rdparty/ippicv/ippicv_lnx/icv
    Intel IPP IW:                sources (2019.0.0)
              at:                /home/gachiemchiep/opt/opencv-3.4.5/opencv-3.4.5/build/3rdparty/ippicv/ippicv_lnx/iw
    Lapack:                      YES (/usr/lib/x86_64-linux-gnu/liblapack.so /usr/lib/x86_64-linux-gnu/libcblas.so /usr/lib/x86_64-linux-gnu/libatlas.so)
    Eigen:                       YES (ver 3.3.4)
    Custom HAL:                  NO
    Protobuf:                    build (3.5.1)

  NVIDIA CUDA:                   YES (ver 10.1, CUFFT CUBLAS NVCUVID)
    NVIDIA GPU arch:             70 75
    NVIDIA PTX archs:

  OpenCL:                        YES (no extra features)
    Include path:                /home/gachiemchiep/opt/opencv-3.4.5/opencv-3.4.5/3rdparty/include/opencl/1.2
    Link libraries:              Dynamic load

  Python 3:
    Interpreter:                 /home/gachiemchiep/opt/miniconda2/envs/opencv_3.4.5_cu10/bin/python3 (ver 3.6.7)
    Libraries:                   /usr/lib/x86_64-linux-gnu/libpython3.6m.so (ver 3.6.7)
    numpy:                       /home/gachiemchiep/opt/miniconda2/envs/opencv_3.4.5_cu10/lib/python3.7/site-packages/numpy/core/include (ver 1.13.3)
    install path:                /cv2/python-3.6

  Python (for build):            /home/gachiemchiep/opt/miniconda2/envs/opencv_3.4.5_cu10/bin/python3

  Java:                          
    ant:                         NO
    JNI:                         /usr/lib/jvm/java-8-openjdk-amd64/include /usr/lib/jvm/java-8-openjdk-amd64/include/linux /usr/lib/jvm/java-8-openjdk-amd64/include
    Java wrappers:               NO
    Java tests:                  NO

  Install to:                    /usr/local
-----------------------------------------------------------------


```

End
