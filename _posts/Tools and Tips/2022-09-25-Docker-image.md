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
# Docker Images
<aside>
This post is originated from [HeejoonLee](https://github.com/HeejoonLee), rewritten by [YoungJ-Baek](https://github.com/YoungJ-Baek)
</aside>

## Supported Frameworks(Datasets)

---

- ORB-SLAM2(Kitti)
- LDSO(Kitti, EuRoC)

## Prerequisites

---

### Docker Engine

[https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

### Dataset

- [Kitti](https://www.cvlibs.net/datasets/kitti/eval_odometry.php)
- [EuRoC](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets#downloads)

### x86_64 System

Supported OS:
- Windows 10(WSL2)
- Ubuntu

<aside>
💡 ARM64 시스템과 호환되지 않음(Apple M1 Pro 확인)
</aside>

## Basic Usage

---

### Docker Image Download

```bash
$ docker pull heejoon1130/slam:0.2
```

<aside>
💡 Image 크기: 3.84GB

</aside>

### Dataset Directory Setup

반드시 아래와 같은 구조로 host system에서 dataset directory를 설정:

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

## Run Docker Container

### Windows(WSL2)

```bash
$ docker run -v [HOST_DATASET_PATH]:/home/slam/datasets \
-e DISPLAY=$DISPLAY \
-it heejoon1130/slam:0.2
```

### Ubuntu

```bash
$ xhost +
$ docker run -v [HOST_DATASET_PATH]:/home/slam/datasets \
-v /tmp/.X11-unix:/tmp/.X11-unix \
-e DISPLAY=$DISPLAY \
-it heejoon1130/slam:0.2
```

<aside>
💡 `xhost +` 는 X server가 모든 client로부터의 연결을 허용하겠다는 의미로 보안이 걱정된다면 Docker container 종료 후 `xhost -` 로 다시 연결을 차단할 수 있음

</aside>

<aside>
💡 HOST_DATASET_PATH: Host system의 경로로 Docker container 내부에서 /home/slam/datasets에 mount되는 경로

</aside>

> 예시) Kitti dataset이 다음과 같이 host에 저장되어있다고 하면: ~/shared/datasets/kitti/odometry_gray/sequences, HOST_DATASET_PATH 는 다음과 같이 설정해야 한다: **~/shared/datasets**
> 

### Run SLAM

최초 실행 시, framework 선택 및 dataset 선택 옵션이 출력 되고, 적절한 옵션을 선택하여 실행.

실행 예시:

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

종료하려면, ctrl+c → q → Enter

## Advanced Usage

---

### Run Docker Container with bash

```bash
$ docker run -v [HOST_DATASET_PATH]:/home/slam/datasets \
-e DISPLAY=$DISPLAY \
-it heejoon1130/slam:0.2 bash
```

다음과 같이 터미널로 출력:

```bash
root@23kflkjasd8f:/home/slam#
```

터미널을 활용하여 직접 소스 수정 이후 빌드하여 실행 가능

## Changes

---

| Date | Version | Image | Description |
| --- | --- | --- | --- |
| 2022/09/20 | 0.1 | heejoon1130/slam:0.1 | 최초 배포. ORB-SLAM2(Kitti), LDSO(Kitti) 지원. Ubuntu 18.04를 base로 제작. |
| 2022/09/23 | 0.2 | heejoon1130/slam:0.2 | LDSO(euroc) 지원. 데이터셋 선택 인터페이스 추가. |
