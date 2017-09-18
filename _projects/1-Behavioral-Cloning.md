---
layout: project
title: Deep Learning - Behavioral Cloning
repository: https://github.com/guiklink/CarND-Behavioral-Cloning-P3
date: February 10, 2015
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/VehicleDetection/logo.png?raw=true
---

<article></article><br/>

# **Behavioral Cloning** 
---

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road


<iframe src="https://player.vimeo.com/video/229937631" width="640" height="320" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/229937631">Autonomous Driving Car</a> from <a href="https://vimeo.com/user43396191">Guilherme Klink</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

<article></article><br/>

<iframe src="https://player.vimeo.com/video/229947155" width="640" height="480" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/229947155">video2</a> from <a href="https://vimeo.com/user43396191">Guilherme Klink</a> on <a href="https://vimeo.com">Vimeo</a>.</p>


## [Go to project description.](https://github.com/guiklink/CarND-Behavioral-Cloning-P3)

### Contents

* **Intro.ipynb**: this notebook contains the step by step of my aproach, showing the manipulation of the images, enhancement of the data and three learning models
* **NN_model**: this notebook was used to train the model of my choice. Here the pipeline loads all the data in the memory at once which is faster for running the trainning epochs, however it may require a lot of GPU memory.
* **NN_generator**: this notebook can also train a model. It uses a data generator with an adjustable batch size, making it viable without a lot of GPU power or for huge datasets, but it is considerably slower.  
* **drive.py**: for driving the car in autonomous mode
* **driving_model.h5py**: contains a trained convolution neural network 
* **Videos**: [Driver View](https://vimeo.com/229937631), [Bird Eye View](https://vimeo.com/229947155)

