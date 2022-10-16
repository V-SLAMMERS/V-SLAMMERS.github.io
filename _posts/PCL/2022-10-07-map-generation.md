---
title: "[PCL] Map Generation"
categories:
    - Point_Cloud_Library
tags:
    - PCL
    - Map
    - Filter
    - Grid
toc : true
toc_sticky: true
comments: true
---

This post briefly introduces the methods used to generate a *minimap* from the point cloud output of a SLAM framework(dso).

All images shown in the post were created using the PCL[TODO: Enter link to pcl website here]. Follow the link for more information on the point cloud library.

The base image used in the post is generated from the *TUM mono sequence_01* dataset using the *dso* framework.

The map generation process mainly consists of three steps: *filtering*, *segmentation*, and *reconstruction*.

## Filtering

The point cloud output of *dso* is a `.pcd` file containing the 3D coordinates of the features detected by the framework.
The coordinates can easily be visualized using the *pcl viewer* that comes with the point cloud library.
However, the raw point cloud output of a SLAM framework usually contains many defects, such as outliers, uneven density, unnecessary points, or missing points, that need to be processed adequately before the set of points can be used as an input to generate a minimap.

The most simple form of processing that can be applied is *filtering*, where we remove points from the point cloud based on certain criteria.

The two mainly used filters available in the point cloud library are: *statistical outlier removal filter* and *voxel grid filter*.

### Statistical Outlier Removal Filter

Parameters: *meanK*, *stddevMulThresh*
(TODO: Explain the purpose of each parameter)

TODO: Place filtered images here

### Voxel Grid Filter

Parameters: *leafSize*

TODO: IMAGE

## Segmentation

Although the filtering process removes unnecessary information from the point cloud, we may still need to derive some meanings from the points instead of purely treating them as a group of points in 3D space.

*Segmentation* tries to obtain higher dimensional information from the points by extracting geometric shapes from the point cloud.

*Plane segmentation* and *region growing segmentation* methods are implemented in the point cloud library.

### Plane Segmentation

Parameters: *distanceThreshold*

TODO: IMAGE

### Region Growing Segmentation

Parameters: *TODO*

TODO: IMAGE

## Reconstruction

With both filtering and segmentation applied, the point cloud now conveys more meaning: the group of points that forms walls, floor and ceiling are easily identified.

The current point cloud, however, is still far from being a usable minimap that we are trying to generate.

One flaw that stands out in the current form is the sheer amount of missing information from the dataset. The walls that should be a single plane are disjointed, and the doorway is no longer recognizable. Such lack of data points may be due to the inherent characteristic of a sparse SLAM framework such as *dso* or the use of a monocular camera which cannot natively capture the depth of each point.

We can compensate for the absence of information using various reconstruction methods. The point cloud library provides the *moving least squares surface reconsturction* API, which we apply to our point cloud:

TODO: IMAGE

Projecting the trajectory of the camera on the point cloud can help us to indentify where the doors are supposed to be:

TODO: IMAGE
