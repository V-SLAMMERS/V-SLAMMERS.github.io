---
title: "[LDSO/Localization] How to match frames with PCD map"
categories:
    - Localization
tags:
    - LDSO
    - Localization
    - Loop Closing
toc : true
toc_sticky: true
comments: true
---
This post is written by [YoungJ-Baek](https://github.com/YoungJ-Baek)
{: .notice--info}

## 1. Goal
For our project's localization, we divide into two parts. First part is how to store the information for matching frames with PCD map. For this part, [YoungJ-Baek](https://github.com/YoungJ-Baek) and [jeonjw25](https://github.com/jeonjw25) will be responsible for the implementation.

## 2. Brainstorming
We have an open discussion for a week, then are able to make two approaches. First one is expanding current LDSO implementation that uses key frames to all frames. Second one is to add additional information for the searching.

## 3. Approach
### 3.1. Expanding key frames to all frames
Simply, we compare current algorithm and modified version.

- As-Is: Key Frames → PCD
- To-Be: All Frames → PCD

In this way, we can earn additional advantage that improves the quality of map. Since the new method uses all frames for map generation, it would be much accurate than the previous way. If we select this method, we can generate BoW and store all frames without any additional implementation. However, we should modify some codes to expand key frames to all frames. In this point, there would be high probability of SW bugs because of previous codes' dependency.

### 3.2. Adding additional information
