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

This is 3D face generation project, I was having so much fun building my face in one click! I write it all in python. 

The steps here are consisted of (1) getting face landmarks, (2) preprocess face image, (3) extracting UV texture, (4) and transfer texture to 3D landmarks. 

# Face landmarks

I use `mediapipe` to get the face landmarks.

