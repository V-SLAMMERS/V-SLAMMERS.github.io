---
title: "[ORB SLAM2/LDSO] Docker Images"
categories:
    - Tools_and_Tips
tags:
    - LDSO
    - DSO
    - ORB SLAM2
    - Docker
toc : true
toc_sticky: true
comments: true
---
This post is originated from [HeejoonLee](https://github.com/HeejoonLee), rewritten by [YoungJ-Baek](https://github.com/YoungJ-Baek)
{: .notice--info}

# Docker Images
## 1. Supported Frameworks(Datasets)
- ORB-SLAM2(Kitti)
- LDSO(Kitti, EuRoC)

## 2. Prerequisites
### 2-1. Docker Engine
[How to install](https://docs.docker.com/engine/install/)

### 2-2. Dataset
- [Kitti](https://www.cvlibs.net/datasets/kitti/eval_odometry.php)
- [EuRoC](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets#downloads)

### 2-3. Supported OS
**x86_64 System**:
- Windows 10(WSL2)
- Ubuntu

💡 This system is not compatible with ARM64 system (observed with Apple M1 Pro)
{: .notice--warning}

### 2-4. Docker Image Download

```bash
$ docker pull heejoon1130/slam:0.2
```
💡 Image Size: 3.84GB
{: .notice}

### 2-5. Dataset Directory Setup
The `datasets` directory should be set up in the host system as in the following structure:

```bash
datasets
└── kitti
    └── odometry_gray
        └── sequences
            ├── 00
            ├── 01
            ├── 02
            ├── 03
            ...
└── euroc
    ├── MH_01_easy
    ├── MH_02_easy
    ├── MH_03_medium
    ├── MH_04_difficult
    ├── MH_05_difficult
    ...
```

## 3. Basic Usage (Run Docker Container)
### 3-1. Windows(WSL2)
```bash
$ docker run -v [HOST_DATASET_PATH]:/home/slam/datasets \
-e DISPLAY=$DISPLAY \
-it heejoon1130/slam:0.2
```

> SLAM frameworks in the Docker image use the *Pangolin* viewer to display the localization and mapping process using *GUI*. An X server must be set up on Windows(`Xming`, ...) and the `DISPLAY` variable correctly set to the host display in order to run GUI applications in WSL2.
{: .notice}

### 3-2. Ubuntu
```bash
$ xhost +
$ docker run -v [HOST_DATASET_PATH]:/home/slam/datasets \
-v /tmp/.X11-unix:/tmp/.X11-unix \
-e DISPLAY=$DISPLAY \
-it heejoon1130/slam:0.2
```

💡 `xhost +` means that X server allows the connection from all clients. If you worry about the security, you can block the connection again via typing `xhost -` after shutting down the Docker container.
{: .notice}

💡 `HOST_DATASET_PATH` is the path to the *datasets* directory in the host system that is mounted in `/home/slam/datasets` inside the Docker container
{: .notice}

<div class="notice" markdown="1">
For example, if we assume that the Kitti dataset is stored in the host as follows:

`~/shared/datasets/kitti/odometry_gray/sequences`

Then, `HOST_DATASET_PATH` should be set in this way, `~/shared/datasets`
</div>

### 3-3. Run SLAM
At the first run, options for selecting framework and dataset will be printed. Then, you can choose appropriate options and run the SLAM.

Run example:
```bash
$ docker run -v ~/shared/datasets:/home/slam/datasets -e DISPLAY=$DISPLAY -it heejoon1130/slam:0.2
[V-SLAMMERS SLAM Docker Image]

Select the SLAM framework to use
Enter 'q' to quit
(1) ORB-SLAM 2
(2) LDSO
Enter a number: 2
---------------------------
Running LDSO
Select a dataset:
(1) Kitti
(2) EuRoC
Enter a number: 1
Select a sequence(Default:00):
['00', '01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21']
Sequence: 00
```

To terminate, input `ctrl + c` → `q` → `Enter`

## 4. Advanced Usage
### 4-1. Run Docker Container with bash

```bash
$ docker run -v [HOST_DATASET_PATH]:/home/slam/datasets \
-e DISPLAY=$DISPLAY \
-it heejoon1130/slam:0.2 bash
```

The output will be like as followed.
```bash
root@23kflkjasd8f:/home/slam#
```

Then, you may modify the source code directly via terminal and test the changes you have made.
