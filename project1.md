---
layout: default
title: "Project 1"
permalink: /project1/
---

# ARM Optimized Motion Estimation 

This project was done for an Embedded Systems class - SENG 440 at UVic. 

https://github.com/aidanmacnichol/SENG440-Motion-Estimation

## Background
The goal of this project was to optimize the "sum of absolute differences" (SAD) operation. This operation is commonly used to compress videos. The general idea is to find regions in a video frame that are much different than the preceding frame. This way only changes from frame to frame are encoded instead of the entire frame itself as often most of the frame is extremely similar. This greatly improves memory and speed when encoding video:

![motion estimation](/assets/images/motion-estimation.png)

from: https://www.eetimes.com/how-video-compression-works/

## Implementations 
A few different approaches were taken for this problem: 

1. Software optimizations
2. Hardware based optimizations

### Hardware based optimizations 
The idea here is to utilize the single instruction, multiple data (SIMD) capabilities of ARM based chips. When computing SAD, the data can be packed into two SIMD vectors and the SAD can be computed 2-4x faster in parallel instead of having to loop through each vector element individually: 

![ARM SIMD](/assets/images/hw_sad.png)

This was implemented in two ways: a Non-reentrant solution (meaning there is no global state and it is safe for simultaneous/threaded execution) and a reentrant solution (state is kept track of with a shared global variable) 

This snippet comes from [GitHub](https://github.com/aidanmacnichol/SENG440-Motion-Estimation).

<pre>
<code>
{% raw %}
curl https://github.com/aidanmacnichol/SENG440-Motion-Estimation/blob/main/hw_sad_nonreentrant.c
{% endraw %}
</code>
</pre>

### Software Optimizations
This was not a very productive approach as gcc already does an amazing job optimizing code when compiled. The methods implemented were: 
- Loop unrolling
- Miscalanous optimizations (predicates, reducing number of branches)


