---
title: "[Path Planning] Algorithm comparison"
categories:
    - Path_Planning
tags:
    - Path Planning
    - A*
    - D*
toc : true
toc_sticky: true
comments: true
---
This post is written by [YoungJ-Baek](https://github.com/YoungJ-Baek)
{: .notice--info}

## 1. Overview
In this post, we compare several path planning algorithms including both grid search based and sampling based algorithms. All of them are based on generated grid map using KITTI dataset and our PCL2GRID module. In conclusion, we could find the shortest path via optimization. In this post, we will check the feasibility via some experiments.

## 2. Algorithm Comparison

### 2.1. Grid Search based algorithms
#### 2.1.1. D* Lite
<p align="center"><img src="/assets/images/posts/d_star_lite.png" height="500px" width="500px"></p>
<!-- ![d_star_lite.png](/assets/images/posts/d_star_lite.png) -->

#### 2.1.2. D*
<p align="center"><img src="/assets/images/posts/d_star.png" height="500px" width="500px"></p>
<!-- ![d_star.png](/assets/images/posts/d_star.png) -->
    
#### 2.1.3. Bidirectional A*
<p align="center"><img src="/assets/images/posts/bidirectional_a_star.png" height="500px" width="500px"></p>
<!-- ![bidirectional_a_star.png](/assets/images/posts/bidirectional_a_star.png) -->
    
#### 2.1.4. Visualization
We did an experiment to measure the impact of visualization. We thought it could be bottleneck, because the image size is too big. As a result, it was a big burden, but not the root cause for the performance drop. You can see the performance comparison by time difference as followed.

1. w/o Visualization : 356.703 sec
2. w/  Visualization : 372.622 sec

#### 2.1.5. Conclusion
Most of all, we could find the right path easily via grid search based algorithms. However, as the algorithm shows higher performance, it has more probability to return inaccurate result, which means it is not the shortest path. Also, the absolute map size has a big influence on the amount of computation. Since our map convert floor map or building map into grid map, it might require huge image size. So, if we decide to use grid search based algorithms, we need to optimize to reduce the amount of computation.

### 2.2. Sampling based algorithms
#### 2.2.1. RRT
We cannot obtain the right path due to the long runtime
{: .notice--warning}

#### 2.2.2. RRT*
We cannot obtain the right path due to the long runtime
{: .notice--warning}

#### 2.2.3. Informed RRT*
We cannot obtain the right path due to the long runtime
{: .notice--warning}

#### 2.2.4. Conclusion
Most of all, we were not able to obtain the right path via sampling based algorithms. To find the root cause of the problem, we did some analysis. Therefore, we found some critical issue of the algorithms. Since the algorithms use randomness and sampling characteristics, it requires collision check process for the every obstacle list for the every node generating steps. So, it requires huge amount of computation if we keep using 2D grid map. Moreover, our map fills black pixels for the all obstacles instead of using boundary line. However, the current code of sampling algorithm registers all the obstacle pixels to plan the right path. In other words, if we apply sampling based algorithms to our current map, we should check all the black pixels for the every node generation process which causes computational bottleneck of the performance.

In conclusion, if we want to use sampling based algorithm, we should generate a whole new map to reduce the computational bottleneck. The examples that open source codes use are minimized or optimized as simple obstacles to check the collision with minimum computation such as rectangular, circle, and so on.     

## 3. Next Step
1. We should simplify the map to move multiple pixels at once or require the map with higher quality that can provides more detailed information
2. We need to think about graph-based map generation if we want to use sampling based algorithm (requires postprocessing after generating PCL map)

## 4. Concern
During the experiment, we found that our goal of path planning is same with global path planning, but we should consider the changes of environment(construction, booth, etc) that users cannot enter. In other words, we should sense the difference of generated map and the user environment. In this case, it is more likely to be local path planning. So, we need to think about turn on the local path planning to find the alternative path, then return to global path planning after localization