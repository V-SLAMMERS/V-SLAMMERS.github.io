---
title: "[10/20] Spot meeting minutes"
categories:
    - Minutes_ARNAVI
tags:
    - minutes
toc : true
toc_sticky: true
comments: true
---
This post is written by [YoungJ-Baek](https://github.com/YoungJ-Baek)
{: .notice--info}

## 1. Purpose
The purpose of this meeting(10/20) is to set goal for next study, 10/24. Attendees for the meeting is [YoungJ-Baek](https://github.com/YoungJ-Baek) and [jeonjw25](https://github.com/jeonjw25).

## 2. Discussed Item
### 2.1. Wrap up about current status
Thanks to [jeongmyunglee](https://github.com/jeongmyunglee), now we can generate three result files for running SLAM, PCL, pose, and BoW files. So, we can reduce our time cost to implement those functions. However, he said that the pose LDSO SLAM generates is not relative poses, but it is absolute poses. We should check this point before implementing such functions. Also, we should define the exact definition of pose in LDSO. We bring three candidates for it during the meeting, absolute position of camera, 3D position of camera (or could it be 2D position?), and cumulative RT values (rotation and translation).

### 2.2. Task
For efficient collaboration, we both agree that we should make a branch for us. Then, we need to implement our functions with arguments. In our last official study, all members agree to implement each parts' tasks with arguments to turn on or off.

We discussed about what to turn on and off with arguments. In conclusion, we could find two candidates for it. First, the range of frames used for localization can be considered. The default is to use only key frames, and our goal is to add an option to use all frames. Second, the test mode of searching map can be considered. However, it would be better to test the function with pregenerated map, which means testing it after LDSO SLAM finishes sounds good. So, we both agree to make a test code for it, not using argument for LDSO command.

### 2.3. Open Discussion about All Frames
Now, we think about iterating all frames if first try iterating only key frames fails. However, is there any better idea? We discussed about this point, and we concluded that it is the trade-off between performance and reliability. At first, we simply think that we can iterate only key frames, and if the score is not enough to match, we can just follow the highest score. However, if we only use the algorithm based on threshold and highest score rule, there is a probability of wrong detection when we initialize the application. It is just a concern, not a fact.

However, if we approach very conventionally which means we use all frames for initialization, we still not be able to sure that the concern is dissolved. On the other hand, we can do more things to prevent root-cause. We can do second comparison within the candidates which have similar but not enough score. In that case, we can expand our iteration into all frames related to candidated key frames.

In conclusion, if our first approach is enough to prevent initialization fail, we do not have to use non key frames. Moreover, if it has only small amount of improvement, the cost for using all frames might be much bigger than the advantage of reliability. However, if it can bring dramatic improvement, we should not hesitate to use them. So, it must be decided by data comparison. Our task should be implementing those things and comparing them.

## 3. To Do
1. Implement using all frames or using key frames only selected by arguments to turn it on/off
    1. It should be included in map generation process
    2. Non key frames should not enter the pose optimization after pixel selection (should generate BoW and store them)
2. Implement initialization using new frame as an input of pregenerated map `(PCL map, pose, BoW)` by independent test code and make a command for it
3. Open discussion for the format of stored `(PCL map, pose, BoW)`

## 4. Action Item for next study
1. Make a Git Branch, Git ignore, and store the result files `(PCL map, pose, BoW)` of key frames
2. Add an argument (it is okay that there's no function)
3. Try to make a test code referring BoW parts at loop detection