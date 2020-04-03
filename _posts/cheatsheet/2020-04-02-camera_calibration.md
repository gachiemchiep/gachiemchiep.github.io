---
title: "Camera calibration"
excerpt: "Some note when learning about Camera Calibration"
categories:
  - cheatsheet
tags:
  - calibration
comments: true
comments_locked: false
toc: true
support: true
published: true
order: 9
author: vugia.truong
---

# What is Camera Calibration?

Geometric camera calibration, estimates the parameters of a lens and image sensor of an image or video camera.
These parameters include *intrinsics*, *extrinsics* and *distortion coefficients*

The relation between *intrinsics* and *extrinsics* can be described as follows.

![structure](/assets/images/cam/calibration_cameramodel_coords.png)

Fig: Camera intrinsics and extrinsics relate (1) (Source: [mathwork](https://jp.mathworks.com/help/vision/ug/camera-calibration.html?lang=en))
In a short-word: *extrinsics* parameters help us turn world coordinates into camera coordinates. 
And *intrinsic* parameters help us turn camera coordinates into pixel coordinate

![structure](/assets/images/cam/calibration_coordinate_blocks.png)
Fig: Camera intrinsics and extrinsics relate (2) (Source: [mathwork](https://jp.mathworks.com/help/vision/ug/camera-calibration.html?lang=en))

Example of Distortion

![Radial Distortion](/assets/images/cam/calibration_radial_distortion.png)
Fig: Radial Distortion (Source: [mathwork](https://jp.mathworks.com/help/vision/ug/camera-calibration.html?lang=en))

![Tangential Distortion](/assets/images/cam/calibration_tangentialdistortion.png)
Fig: Tangential Distortion (Source: [mathwork](https://jp.mathworks.com/help/vision/ug/camera-calibration.html?lang=en))

# Camera Projection 

Camera projection = convert 3d point (X, Y, Z) into camera's pixel coordinate (x, y)

![Camera projection](/assets/images/cam/cam_projection_1.png)

Forward projection == convert World coords into pixel coords.

![Camera projection](/assets/images/cam/cam_projection_2.png)

Backward projection = convert pixel coords into world coords

![Camera projection](/assets/images/cam/cam_projection_3.png)

The relation between foward projection and *extrinsics* (Mext) and *extrinsics* (Mint)

![Camera projection](/assets/images/cam/cam_projection_4.png)

In matrix manipulation format

![Camera projection](/assets/images/cam/cam_projection_8.png)

## Camera extrinsics

The *extrinsics* is the result of a *rotation* and a *translation*

![Camera projection](/assets/images/cam/cam_projection_5.png)

![Camera projection](/assets/images/cam/cam_projection_6.png)

## Camera intrinsics

The *intrinsics* is the result of a *affine Transformation* (Maff) and a *perspective projection*

![Camera projection](/assets/images/cam/cam_projection_7.png)

The *perspective projection*

![Camera projection](/assets/images/cam/cam_projection_9.png)

Re-write in matrix manipulation

![Camera projection](/assets/images/cam/cam_projection_10.png)

The *affine Transformation*

![Camera projection](/assets/images/cam/cam_projection_11.png)

The camera intrinsics is a 3x3 matrix as follow

![Camera projection](/assets/images/cam/cam_projection_12.png)

# Camera Projection implementation

These code are taken from intel realsensen source code. For detail see [https://github.com/IntelRealSense/librealsense/blob/master/include/librealsense2/rsutil.h](https://github.com/IntelRealSense/librealsense/blob/master/include/librealsense2/rsutil.h)

This is only used for check the matrix manipulation in above section

## Camera extrinsics

```Cpp
typedef struct rs2_extrinsics
{
    float rotation[9];    /**< Column-major 3x3 rotation matrix */
    float translation[3]; /**< Three-element translation vector, in meters */
} rs2_extrinsics;
```

## Camera intrinsics

```Cpp
typedef struct rs2_intrinsics
{
    int           width;     /**< Width of the image in pixels */
    int           height;    /**< Height of the image in pixels */
    float         ppx;       /**< Horizontal coordinate of the principal point of the image, as a pixel offset from the left edge */
    float         ppy;       /**< Vertical coordinate of the principal point of the image, as a pixel offset from the top edge */
    float         fx;        /**< Focal length of the image plane, as a multiple of pixel width */
    float         fy;        /**< Focal length of the image plane, as a multiple of pixel height */
    rs2_distortion model;    /**< Distortion model of the image */
    float         coeffs[5]; /**< Distortion coefficients */
} rs2_intrinsics;
```

or inside Ros's senser_msgs. Please be noted that matlab use different format with ros.

```bash
# Intrinsic camera matrix for the raw (distorted) images.
#     [fx  0 cx]
# K = [ 0 fy cy]
#     [ 0  0  1]
# Projects 3D points in the camera coordinate frame to 2D pixel
# coordinates using the focal lengths (fx, fy) and principal point
# (cx, cy).
float64[9]  K # 3x3 row-major matrix
```

## Camera projection

*Pixel coords* into *Camera Coords* 

```Cpp
static void rs2_deproject_pixel_to_point(float point[3], const struct rs2_intrinsics * intrin, const float pixel[2], float depth)

```

*Camera Coords* into *Pixel coords* 

```Cpp
static void rs2_project_point_to_pixel(float pixel[2], const struct rs2_intrinsics * intrin, const float point[3])
```

*Camera 1 Coords* -> *World Coords* -> *Camera 2 Coords* 

```Cpp
static void rs2_transform_point_to_point(float to_point[3], const struct rs2_extrinsics * extrin, const float from_point[3])

```

# Reference

1. [https://www.scratchapixel.com/lessons/3d-basic-rendering/computing-pixel-coordinates-of-3d-point/perspective-projection](https://www.scratchapixel.com/lessons/3d-basic-rendering/computing-pixel-coordinates-of-3d-point/perspective-projection)
2. [Camera Projection](http://www.cse.psu.edu/~rtc12/CSE486/lecture12.pdf)
3. [Camera Projection 2](http://www.cse.psu.edu/~rtc12/CSE486/lecture13.pdf)
4. [ros information](http://docs.ros.org/melodic/api/sensor_msgs/html/msg/CameraInfo.html)
5. [https://github.com/IntelRealSense/librealsense/blob/master/include/librealsense2/rsutil.h](https://github.com/IntelRealSense/librealsense/blob/master/include/librealsense2/rsutil.h)
6. [Realsense Projection-in-RealSense-SDK-2.0](https://github.com/IntelRealSense/librealsense/wiki/Projection-in-RealSense-SDK-2.0)

End