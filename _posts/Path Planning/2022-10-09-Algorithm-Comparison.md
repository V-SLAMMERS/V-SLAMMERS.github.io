---
title: "[Path Planning] Algorithm comparison"
categories:
    - Path_Planning
tags:
    - Path Planning
toc : true
toc_sticky: true
comments: true
---
This post is written by [YoungJ-Baek](https://github.com/YoungJ-Baek)
{: .notice--info}

# Brainstorm

- Concern
    - 목적은 Global path-planning과 부합하나, map 생성 이후 user 사용 환경에서 변화된 환경(공사, 부스 설치 등)에 의해 갈 수 없는 길이 존재 가능
    - 위와 같은 경우 Local path-planning으로 회피해 그 위치에서 localize 후 Global path-planning으로 다시 진입해야 할 것으로 판단됨
- Issue
    - Obstacle이 Boundary line이 아니라 모두 Black으로 되어 있으면, 연산량이 많아진다. 현재 코드는 Obstacle pixel들을 등록해 Path-planning을 진행하고 있다.
    - Sampling based algorithm들(RRT based)은 현재 open source들을 살펴봤을 때, randomness + sampling 특성이 합쳐져 모든 node 생성 과정에 collision check를 모든 obstacle list에 대해 진행한다. 따라서, 현재 map에 이 open source들을 적용한다면 매 node 생성 시 모든 black pixel들을 확인해 연산량이 방대해진다.

## 1. Overview
In this post, we compare several path planning algorithms including both grid search based and sampling based algorithms. All of them are based on generated grid map using KITTI dataset and our PCL2GRID module. In conclusion, we could find the shortest path via optimization. In this post, we will check the feasibility via some experiments.

## 2. Algorithm Comparison

### 2.1. Grid Search based
1. D* Lite
<p align="center"><img src="/assets/images/posts/d_star_lite.png" height="100px" width="100px"></p>
<!-- ![d_star_lite.png](/assets/images/posts/d_star_lite.png) -->
    
- D*
<p align="center"><img src="/assets/images/posts/d_star.png" height="100px" width="100px"></p>
<!-- ![d_star.png](/assets/images/posts/d_star.png) -->
    
- Bidirectional A*
<p align="center"><img src="/assets/images/posts/bidirectional_a_star.png" height="100px" width="100px"></p>
<!-- ![bidirectional_a_star.png](/assets/images/posts/bidirectional_a_star.png) -->
    
- Result
    - w/o Visualization : 356.703 sec
    - w/   Visualization : 372.622 sec
- Conclusion
    1. Grid Search 기반 알고리즘들은 경로를 탐색하는데 어려움은 없음
    2. 성능이 증가한 알고리즘일수록 최단 거리가 아닌 확률 증가
    3. 절대적인 map size가 연산량에 큰 영향을 미침

- Sampling based
    - RRT
        - Runtime이 너무 길어 path 추정 불가
    - RRT*
        - Runtime이 너무 길어 path 추정 불가
    - Informed RRT*
        - Runtime이 너무 길어 path 추정 불가
    - Conclusion
        1. 위에 Issue에서 언급했듯이, node 생성 시 모든 obstacle(black pixel)에 대해 장애물 여부를 확인하기 때문에, 연산량 줄일 수 있는 map이 필요
        2. 현재 open source에서 제공하는 예제는 장애물들을 단순화(rectangular, circle, etc.)해 적은 연산으로 collision check가 가능함
        

# Next Step

1. N pixel 단위로 이동이 가능하게 obstacle 단순화 or map 정보를 더 많이 필요(장애물 최소 거리)
2. 그래프 형식으로 path-planning이 가능하도록 map postprocessing