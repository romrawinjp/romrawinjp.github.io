---
title: "3D Face Generation"
date: "2023-10-12"
author: Romrawin Chumpu
layout: "post"
tags: ["work", "project", "code"]
env: "production"
ShowWordCount: true
# tocopen: true
ShowShareButtons: false
---

This is 3D face generation project, I was having so much fun building my face in one click! I programmed all in python. Back in 2021 (two years prior learning Blender), I did the texture translation pipeline from OpenCV and PyTorch3D + math equations visualized from my ideas (which I thought it should be working). 

Steps here are consisted of (1) getting face landmarks, (2) preprocess face image, (3) extracting UV texture, (4) and transfer texture to 3D landmarks. The code is workable in Google Colab.  

# Face Landmarks Detection

I used package called `mediapipe` to get the face landmarks. <a href="https://developers.google.com/mediapipe/solutions/vision/face_landmarker"> MediaPipe Face Landmarker </a> gives 468 landmarks output in 3-dimensional spaces; these are basically x, y, z coordinates. I was experimenting with `dlib` (68 landmarks) and found that the more landmarks we get, the better we can play with face texture. Here is the <a href="https://github.com/googlesamples/mediapipe/blob/main/examples/face_landmarker/python/%5BMediaPipe_Python_Tasks%5D_Face_Landmarker.ipynb">ipynb demo </a> for face landmark detection. 

![MediaPipe Landmarks](https://developers.google.com/static/mediapipe/images/solutions/examples/face_landmark.png)
How cool are these landmarks!