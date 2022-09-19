---
title: "[LDSO/Code Review] Introduction"

categories:
    - LDSO
tags:
    - LDSO
    - DSO
    - Code Review
last_modified_at: 2022-09-19T21:50:00
toc : true
toc_sticky: true
comments: true
---

# LDSO Code Review

## Goal

  In this chapter, we are going to review the LDSO code and understand it in depth. Our final goal is to modularize the LDSO code and to use some parts of it. Most of all, we need localization function for our project.

 

## Role

  Our team members divide the code into 4 sections at first, and are responsible for each part. We select 4 granularities that needs to be studied with much effort and background knowledge, PixelSelector2, Undistort, CoarseInitializer, and CoarseTracker. 4 members who are responsible for those parts are as follows.

- PixelSelector2 - [YoungJ-Baek](https://github.com)
- Undistort - [jeonjw25](https://github.com/jeonjw25)
- CoarseInitializer - [jeongmyunglee](https://github.com/jeongmyunglee)
- CoarseTracker - [HeejoonLee](https://github.com/HeejoonLee)

## Reference

1. [LDSO repository](https://github.com/tum-vision/LDSO)

2. [DSO repository](https://github.com/JakobEngel/dso)

3. [Parameter explanation of DSO](https://tongling916.github.io/tags/#DSO)

4. [DSO Code Review](https://alida.tistory.com/45)
