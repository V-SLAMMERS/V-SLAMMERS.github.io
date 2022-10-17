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
Our next approach is register all the non key frames into neighbor key frames. All we have to do is making BoW of non key frames. Then, we will store the frame with `BoW + index` format. Finally, we can detect the similar frame while localization, and initialize the position with the index marked in frame.

In this way, we do not have to convert all frames' feature into PCD. So, we can save our time by reducing additional computation. Also, we do not have to change the code. All we have to do is adding some functions. It is still hard work, but we do not have to worry about SW bugs of former code. However, implementation could be very complex. The figure as followed is the concept image of data structure in DB.

<p align="center"><img src="/assets/images/posts/data_structure.png" height="500px" width="500px"></p>

## 4. Conclusion
We both agree that the most of searching is operated in DB of key frames. In other words, searching non key frames should be optional and work as an error prevention code, nice to have. Moreover, the additional advantage of the first approach cannot solve the root cause of our concern. Even if we use all frames rather than sampled key frames, we cannot detect the smooth wall. It is our further concern. So, we decide to select the second approach. It is more reasonable to use key frames for key, and non key frames for value.

Furthermore, we discuss how to optimize the performance. We can compare the key frames first. If it shows high score enough to decide to localize, finish the algorithm. On the other hand, if the highest scored key frame does not show enough score, filter the candidate frames and then expand their non key frames to search right frame.

## 5. Summary
Summary of comparison between two approaches and the flowchart of the second algorithm will be added. TBD.