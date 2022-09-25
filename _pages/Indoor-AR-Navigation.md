---
title: "Indoor AR Navigation project based on SLAM technology"
permalink: /categories/Indoor_AR_Navigation/
layout: category
author_profile: true
taxonomy: Indoor-AR-Navigation
---
***Indoor AR Navigation** aims to build a complete SLAM-based navigation system that provides the users with the ability to (1)generate a 3D map of their surroundings, (2)track their position in real-time, and (3)navigate the area with the help of on-screen AR instructions.*

# PROJECT SUMMARY

*Generating an optimal path from point A to point B is easily achieved today using a simple map application on a smartphone with the help of GPS and satellite technologies. However, the same task becomes non-trivial when our desired path lies indoors. The navigation issue most people face today does not involve finding a way to get to a place; the real problem starts when they encounter the maze-like inner structure of the building and realize that there is no GPS to guide them to their destination. The difficulty of finding your way indoors will only intensify with the ever-increasing number of complex modern facilities, such as malls, universities, etc.*

*Many companies are providing indoor AR navigation services to deal with such a problem. These navigation services include a highly-detailed indoor map generated using visual equipment(stereo camera, RGB camera, LIDAR sensor) that can directly capture depth information. Although such a method provides accurate service, the high equipment cost makes many individuals think twice before investing in indoor navigation. It also means the availability of the map will be low; there is a high chance the place you are going to does not provide an indoor navigation service.*

***Indoor AR Navigation** solves the same problem in an innovative way. **Indoor AR Navigation** only relies on smartphones to build a complete indoor navigation system. We also maintain a database of user-uploaded indoor maps to deal with the problem of map availability.*

*Most smartphones released recently include processors and cameras that are powerful enough to run SLAM. By adding an extra framework to display in AR the optimized paths based on SLAM-generated maps, we provide a user-friendly and practical interface for indoor navigation.*

*Our application also allows users to share the map that they generated. With the power of crowd-sourcing, the availability and accuracy of indoor maps will scale exponentially as the number of users of our application increases.*

---

# PROJECT ARCHITECTURE

![flowchart.jpg](assets/images/posts/flowchart.jpg)

  *Our architecture is composed of Map Generation, Path Planning and AR Navigation parts. In the perspective of development, architecture can be designed with development and application parts.*

  *Main function of development part is generating map with VSLAM. We generate internal map with video, and convert point cloud map into grid map. Then, localize and do path planning at the application part. Finally, we can reach the goal of application part, AR navigating.*
