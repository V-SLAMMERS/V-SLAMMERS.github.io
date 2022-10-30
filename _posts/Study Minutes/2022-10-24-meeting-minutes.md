---
title: "[10/24] Study minutes"
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
The purpose of this study(10/24) is interim check and to set goal for next study. Attendees for the meeting is [YoungJ-Baek](https://github.com/YoungJ-Baek), [jeonjw25](https://github.com/jeonjw25), [jeongmyunglee](https://github.com/jeongmyunglee), and [HeejoonLee](https://github.com/HeejoonLee).

## 2. Discussed Item
Today, we have discussed about the test code for loop closing. Our current stage is to implement localization, and some techniques used for loop closing is mandatory for it. So, we did deep dive about loop closing, and decided to develop the test code for loop closing. Our code will be composed of 7 steps.

1. Load ORB Vocabulary
2. Load DBoW Database
3. Load Image
    - Image should be non key frame which is close to the loop detection
    - Image should be loaded via `ImageFolderLoader`
4. Convert Image to Frame
5. Calculate BoW via Feature Detection
6. Detect Loop Closing via BoW
    - This will be our first goal
7. Loop Correction
    - If it is needed

## 3. Action Item
Today, we decide to develop test code for loop closing. At this time, we decide to make 4 different codes for trials and errors. So, all the four members will develop test code for loop closing.

## 4. Next Agenda
Next time, we will check the status of test code development. Our first goal is to detect loop closing with the test code. So, we will compare each codes and combine them into our common code.