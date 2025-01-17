---
layout: default
title: "Project 1"
permalink: /project1/
---

# ARM Optimized Motion Estimation 

This project was done for an Embedded Systems class - SENG 440 at UVic. 

## Background
The goal of this project was to optimize the "sum of absolute differences" (SAD) operation. This operation is commonly used to compress videos. The general idea is to find regions in a video frame that are much different than the preceding frame. This way only changes from frame to frame are encoded instead of the entire frame itself as often most of the frame is extremely similar. This greatly improves memory and speed when encoding video:

![motion estimation](/assets/images/motion-estimation.png)
from: https://www.eetimes.com/how-video-compression-works/

