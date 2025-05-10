---
layout: default
title: "Real Time Drone Trajectory Prediction"
---

# Real Time Drone Trajectory Analysis 
This project was done as the capstone for my Masters degree in colaboration with MeasuredAI Corp. The basic idea was to build a complete system that could identify, track and predict drone movements all in real time.

<video width="100%" controls>
  <source src="/assets/videos/drone_GUI.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>


# Skills
- C++ & Python
- Software Runtime Optimization
- PCIE
- DSP & Algorithms
- AutoCAD
- ML: Quantization and YOLO

## Background

There are three major components to the system: 
1. Physical Pan/Tilt assembly.
2. Inference using a Raspberry Pi with a Hailo Edge ML accelerator. 
3. Predictive analysis using a Kalman filter.

The overall block diagram is shown below: 

![telescope-me](/assets/images/drone_block_diagram.jpg)

## Model Creation
To do this we created a dataset and labelled around 2000 images using roboflow. These are annotated pictures of drones including a bounding box and keypoints for each motor. From there we trained a YOLO v8 model to do the keypoint detection.

## Inference 
To run the model a Hailo Raspberry Pi AI hat was used. Our trained YOLO model was compiled and quantized to run on the hardware and post processing for decoding and non maximal suppression was implemented on the CPU side as our model had to be truncated after the convolutional layers to fit on the HW. The screenshot below shows where the model was truncated on its graph.

![model truncation](/assets/images/model_netron.PNG)

Significant work was done to optimize inference to hit the requirment of 30 HZ realtime operation. Some tools used were:

- flamegraphs
- speedscope
- valgrind
- perf

## Pan Tilt

![pan-tilt](/assets/images/pan-tilt.jpg)

In order to better track drones a pan/tilt system was designed in AutoCAD and 3D printed. It was controlled using a basic PC985 Driver and controlled on a separate thread on the software side of things. 

The movement is controlled using a basic PID controller:

![PID](/assets/images/pid.jpg)

## Predictive Analysis 
A Kalman filter was used for the predictive drone analysis. Using some known distances in our test setup we were able to tune the filter to provide predictions in real measurments accurate to ~3 cm in a controlled test enviroment: 

![Test Setup](/assets/images/kalman.jpg)