---
title: "[ORB SLAM2] ORB SLAM2 Installation Guide"
categories:
    - Tools and Tips
tags:
    - ORB SLAM2
toc : true
toc_sticky: true
comments: true
---
This post is originated from [HeejoonLee](https://github.com/HeejoonLee), rewritten by [YoungJ-Baek](https://github.com/YoungJ-Baek)
{: .notice--info}

# ORB-SLAM 2 Installation Guide
This post is about how to build and run ORB SLAM2. If you follow the following guidance in the orders, you will not face any troubles with ORB SLAM2 installation. Special thanks to [HeejoonLee](https://github.com/HeejoonLee), our team provide Docker images for easy way in our blog. You can easily run ORB SLAM2 in [here](https://v-slammers.github.io/tools_and_tips/Docker-image/), then you can install it in your local to modify or do other things.

## 1. OS
For now, we have checked for two OS as followed. If you are trying to use other OS, please refer the official documents or other guidance.

- Ubuntu(Debian, Linux)
- WSL2

## 2. Prerequisites
### 2.1. Kitti Dataset Download
💡 Dataset requires about 22GB that means it requires much time to download, so it would be better to download first and then move to the next step.
{: .notice--info}

First, you can follow the link as followed. Then, you can download the dataset via [Download odometry data set (grayscale, 22 GB)](http://www.cvlibs.net/datasets/kitti/user_login.php)

[The KITTI Vision Benchmark Suite](http://www.cvlibs.net/datasets/kitti/eval_odometry.php)

⚠ You need to sign up before downloading.
{: .notice--warning}

### 2.2. Update Package Info
```bash
~$ sudo apt update && sudo apt upgrade
```

### 2.3. OpenCV
Reference: [How to install OpenCV on Ubuntu 18.04](https://linuxize.com/post/how-to-install-opencv-on-ubuntu-18-04/)

#### 2.3.1. Install Dependency
💡 You need to change the last `libdc1394-22-dev` into `libdc1394-dev` if you are using Ubuntu 22.04 or upper version.
{: .notice--primary}
```bash
~$ sudo apt install build-essential cmake git pkg-config libgtk-3-dev \
 libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
 libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
 gfortran openexr libatlas-base-dev python3-dev python3-numpy \
 libtbb2 libtbb-dev libdc1394-22-dev
```

#### 2.3.2. Download OpenCV, OpenCV_contrib repository
```bash
~$ git clone https://github.com/opencv/opencv.git
~$ git clone https://github.com/opencv/opencv_contrib.git
```

#### 2.3.3. OpenCV, OpenCV_contrib 3.4.9 branch checkout
💡 Note that OpenCV 4.x version is not compatible with ORB SLAM2. Please keep in mind that OpenCV 4.x will be set if you install with default option.
{: .notice--warning}

```bash
~$ cd opencv
~/opencv$ git checkout 3.4.9
~/opencv$ cd ../opencv_contrib
~/opencv_contrib$ git checkout 3.4.9
```

#### 2.3.4. CMake
```bash
~/opencv_contrib$ cd ../opencv
~/opencv$ mkdir build && cd build
~/opencv/build$ cmake -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ..
```

#### 2.3.5. Build OpenCV
```bash
~/opencv/build$ make -j8
```

#### 2.3.6. Install OpenCV
```bash
~/opencv/build$ sudo make install
```

### 2.4. Eigen3
```bash
~$ sudo apt install libeigen3-dev
```

### 2.5. Pangolin
Reference1: [https://blog.csdn.net/Robert_Q/article/details/121690089](https://blog.csdn.net/Robert_Q/article/details/121690089)

Reference2: [https://woojjang.tistory.com/52](https://woojjang.tistory.com/52)

#### 2.5.1. Download Pangolin repository
```bash
~$ git clone --recursive https://github.com/stevenlovegrove/Pangolin.git
~$ cd Pangolin
```

#### 2.5.2. Install Dependency
```bash
~/Pangolin$ ./scripts/install_prerequisites.sh recommended
```
#### 2.5.3. Pangolin v0.5 branch checkout
💡 Note that ORB SLAM2 is not compatible with Pangolin v0.6 or upper version. Please keep in mind that Pangolin v0.8 will be installed if you install with default option.
{: .notice--warning}

```bash
~/Pangolin$ git checkout v0.5
```

#### 2.5.4. Modify Pangolin source code
1. **Pangolin/CMakeModules/FindFFMPEG.cmake**

- line # 63, 64

As-is:
```cpp
  sizeof(AVFormatContext::max_analyze_duration2);
}" HAVE_FFMPEG_MAX_ANALYZE_DURATION2
```

To-be:
```cpp
  sizeof(AVFormatContext::max_analyze_duration);
}" HAVE_FFMPEG_MAX_ANALYZE_DURATION
```

2. **Pangolin/src/video/drivers/ffmpeg.cpp**

- Add define phrase as followed right above the `namespace pangolin` at line number 37.
```cpp
#define CODEC_FLAG_GLOBAL_HEADER AV_CODEC_FLAG_GLOBAL_HEADER
```

- line # 78, 79

As-is:
```cpp
	TEST_PIX_FMT_RETURN(XVMC_MPEG2_MC);
	TEST_PIX_FMT_RETURN(XVMC_MPEG2_IDCT);
```

To-be:
```cpp
#ifdef FF_API_XVMC
	TEST_PIX_FMT_RETURN(XVMC_MPEG2_MC);
	TEST_PIX_FMT_RETURN(XVMC_MPEG2_IDCT);
#endif
```

- line # 101-105

As-is:
```cpp
	TEST_PIX_FMT_RETURN(VDPAU_H264);
	TEST_PIX_FMT_RETURN(VDPAU_MPEG1);
	TEST_PIX_FMT_RETURN(VDPAU_MPEG2);
	TEST_PIX_FMT_RETURN(VDPAU_WMV3);
	TEST_PIX_FMT_RETURN(VDPAU_VC1);
```

To-be:
```cpp
#ifdef FF_API_VDPAU
	TEST_PIX_FMT_RETURN(VDPAU_H264);
	TEST_PIX_FMT_RETURN(VDPAU_MPEG1);
	TEST_PIX_FMT_RETURN(VDPAU_MPEG2);
	TEST_PIX_FMT_RETURN(VDPAU_WMV3);
	TEST_PIX_FMT_RETURN(VDPAU_VC1);
#endif
```

- line # 127

As-is:
```cpp
	TEST_PIX_FMT_RETURN(VDPAU_MPEG4);
```

To-be:
```cpp
#ifdef FF_API_VDPAU
	TEST_PIX_FMT_RETURN(VDPAU_MPEG4);
#endif
```

3. **Pangolin/include/pangolin/video/drivers/ffmpeg.h**

- Add define phrase as followed right above the line number 53, `namespace pangolin`.
```cpp
#define AV_CODEC_FLAG_GLOBAL_HEADER (1 << 22)
#define CODEC_FLAG_GLOBAL_HEADER AV_CODEC_FLAG_GLOBAL_HEADER
#define AVFMT_RAWPICTURE 0x0020
```

4. **Pangolin/src/display/device/display_x11.cpp**

- line # 115

As-is:
```cpp
GLXFBConfig* fbc = glXChooseFBConfig(display, DefaultScreen(display), visual_attribs, &fbcount);
```

To-be:
```cpp
// GLXFBConfig* fbc = glXChooseFBConfig(display, DefaultScreen(display), visual_attribs, &fbcount);
cm
```

- line # 177

As-is:
```cpp
{
	throw std::runtime_error("Pangolin in X11: ...");
}
```

To-be:
```cpp
{
	// throw std::runtime_error("Pangolin in X11: ...");
}
```

#### 2.5.5. Build Pangolin
```bash
~/Pangolin$ cmake -B build
~/Pangolin$ cmake --build build
```

#### 2.5.6. Install Pangolin
```bash
~/Pangolin$ cd build
~/Pangolin/build$ sudo make install
```

## 3. ORB SLAM 2
### 3.1. Download ORB-SLAM 2 repository
```bash
~$ git clone https://github.com/raulmur/ORB_SLAM2.git ORB_SLAM2
```

### 3.2. Modify ORB-SLAM 2 source code
1. **ORB_SLAM2/include/System.h**
- Add `#include <unistd.h>` right below the line number 23, `#define SYSTEM_H`
As-is:
```cpp
#ifndef SYSTEM_H
#define SYSTEM_H

#include <string>
```

To-be:
```cpp
#ifndef SYSTEM_H
#define SYSTEM_H

#include <unistd.h>
#include <string>
```

2. **ORB_SLAM2/include/LoopClosing.h**
- line # 50

As-is:
```cpp
	Eigen::aligned_allocator<std::pair<const KeyFrame*, g2o::Sim3> > > KeyFrameAndPose;
```

To-be:
```cpp
	Eigen::aligned_allocator<std::pair<KeyFrame* const, g2o::Sim3> > > KeyFrameAndPose;
```

### 3.3. Build ORB-SLAM 2
```bash
~$ cd ORB_SLAM2
~/ORB_SLAM2$ ./build.sh
```

## 4. Xming(Optional for WSL2 environment)
If you are trying to run the software with GUI in WSL2 environment, you have to install Xserver in your Windows. More details are in the following link.
[https://evandde.github.io/wsl2-x/](https://evandde.github.io/wsl2-x/)

## 5. Run ORB-SLAM 2
ex) Kitti sequence 00

```bash
~/ORB_SLAM2$ ./Examples/Monocular/mono_kitti Vocabulary/ORBvoc.txt Examples/Monocular/KITTI00-02.yaml [kitti dataset 경로]/odometry_gray/sequences/00
```