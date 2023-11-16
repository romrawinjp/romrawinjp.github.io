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

First we need to install `mediapipe` package.

    !pip install mediapipe

Also install `pytorch3d`. We will use PyTorch3D for a fun visualization! 
    
    !pip install "git+https://github.com/facebookresearch/pytorch3d.git"

Note: Installing `pytorch3d` would take a little while depending on your machine.

## Import Library

We will import all libraries as following:

    import os
    import cv2
    import torch
    import pandas as pd
    import numpy as np
    import mediapipe as mp
    from google.colab.patches import cv2_imshow
    from tqdm.notebook import tqdm

    # pytorch3d
    from pytorch3d.io import load_obj, save_obj
    from pytorch3d.structures import Meshes
    from pytorch3d.utils import ico_sphere
    from pytorch3d.ops import sample_points_from_meshes, add_pointclouds_to_volumes
    from pytorch3d.loss import (
        chamfer_distance,
        mesh_edge_loss,
        mesh_laplacian_smoothing,
        mesh_normal_consistency,
    )
    from pytorch3d.vis.plotly_vis import AxisArgs, plot_batch_individually, plot_scene
    from pytorch3d.vis.texture_vis import texturesuv_image_matplotlib
    from pytorch3d.renderer import (
        look_at_view_transform,
        FoVPerspectiveCameras,
        OpenGLPerspectiveCameras,
        PointLights,
        DirectionalLights,
        Materials,
        RasterizationSettings,
        MeshRenderer,
        MeshRasterizer,
        SoftPhongShader,
        TexturesUV,
        TexturesVertex,
        look_at_rotation
    )

    # matplotlib
    from mpl_toolkits.mplot3d import Axes3D
    import matplotlib.pyplot as plt
    import matplotlib as mpl
    mpl.rcParams['savefig.dpi'] = 80
    mpl.rcParams['figure.dpi'] = 80

    # Set the device
    if torch.cuda.is_available():
        device = torch.device("cuda:0")
    else:
        device = torch.device("cpu")
        print("WARNING: CPU only, this will be slow!")