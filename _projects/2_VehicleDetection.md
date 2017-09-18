---
layout: project
title: Vehicle Detector
repository: https://github.com/guiklink/CarND-Vehicle-Detection
date: September 22, 2014
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/VehicleDetection/logo.png?raw=true
---

<article></article><br/>

# Vehicle Detection Project {#index-ignore}

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.


<iframe src="https://player.vimeo.com/video/233159995" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/233159995">Vehicle Detection</a> from <a href="https://vimeo.com/user43396191">Guilherme Klink</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

[//]: # (Image References)
[image1]: ./output_images/car_not_car.png
[image2]: ./output_images/hog.png
[image3]: ./output_images/sliding_windows.png
[image4]: ./output_images/sliding_windows_HSV.png
[image5]: ./output_images/sliding_windows_imp.png
[image6]: ./output_images/heat_map.png


## [Go to project description.](https://github.com/guiklink/CarND-Vehicle-Detection)

## Contents
* [Project Video](https://vimeo.com/233159995)
* [Train_Model](Train_Model.ipynb): notebook used for training a SVC.
* [Video_Pipeline](Video_Pipeline.ipynb): notebook used to produce the output video.
* [Trials](Trials.ipynb): notebook used to test different parameters for training.
* [auxiliary.py](auxiliary.py): contains all the functions used in the project.
* [model.p](model.p): a pickle file containing the trained model for HSV images and the parameters used.
* [model_YCrCb.p](model_YCrCb.p): a pickle file containing the trained model for YCrCb and the parameters used.

