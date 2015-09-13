---
layout: project
title: Flying Robots
repository:
date: February 10, 2015
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/pj2_logo_small.JPG?raw=true
---

<article></article><br/>

# Introduction
Coming from an undergrad in Computer Science, a work experience in developing and market studies, I challenged myself in getting in the world of UAV's, by studying their dynamics, physics and control techniques. I believe that a daily collaboration between humans and UAV's, would be quite interesting. At this point, I do not expect drones to be helping us to carry burdensome loads of weight, the energy and aerodynamics for that is not easily available, or grasping complicated objects, after all that could be years of someone's PhD thesis in robot manipulation. Instead, what if I could use some Control, Artificial Intelligence and Computer Vision techniques to transform my UAV into something like this:

![navi_following](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/navi_following.jpg?raw=true) ![navi_friend](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/navi_friend.jpg?raw=true)  
![navi_targeting](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/navi_targeting.jpg?raw=true) ![navi_hint](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/navi_hint.jpg?raw=true)  

In the series "The Legend of Zelda: Ocarina of Time" for N64 (Nintendo - 1998), the fairy Navi assisted the protagonist Link on his quest to save a forsaken land by following him around, giving another view of perspective, recognizing friends and foes, conquering treacherous dungeons or even giving useful advices.

Now, what if we could bring that to the real world? Let's change the fairy wings for rotors and we have a quadrotor, that could follow us around, stay in our house or office and assist us with our daily quests (which diffently than Link I hope is not vanquishing goblins and shadow creatures). What if my fairy based drone could monitor my apartment, recognizing the people inside, film me spiking a volleyball from a nice angle, carry a letter to a co-worker whose desk is across the hall or even access the SIRI service from my iPhone and through a small on board speaker reminds me from my daily agenda (after seeing me goofing off on the couch). In some way, this is not entirelly new as there are companies doing something along these lines, still this is my main motivation in working with UAV's. 
 
Achieving this idea is a long run and for that purpose I will try to follow a roadmap of small projects (that will probably go on even after my masters program) covering each specific area of knowledge that in my opinion will help me to my goal.    

## My Quadrotors

### [Crazy Flie](http://www.bitcraze.se/crazyflie/)
![Crazy_Flie](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/crazy_flie.JPG?raw=true)  
Weighing less than 20g and totally open source this nano quadrotor is a great startup for any quadrotor aficionado. Its parts are very cheap and it is so light that is harmless if collides to something.  

### [AR Parrot 2.0](http://ardrone2.parrot.com/)
![AR_parrot](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/ar_parrot.JPG?raw=true)  
Due to its popularity, well documented SDK and lots of [ROS](http://www.ros.org/) packages available, I would say the that AR Parrot series is a good choice for those who wants to have a HD camera on board transmitting through wifi without having to build their own quadrotor. In addition, the downwards camera provides an amazing stability algorithm in ambients with good illumination, what is perfect for indoor applications.


# 1 - Dynamics and Simulation

## Intro
When customizing quadcopters, by adding gadgets, increasing motors or changing its body shape, it is important to understand at a fundamental level their dynamics, in order to get the best performance of the controller that will be implemented. On this first project my goal was to understand the physics of quadrotors and how that is applied in the ROS simulator "Hector Quadrotor", so it can be altered and used to simulate different kinds of flying drones and get an idea of how they will perform before actually building them.

I started by making a Hector Quadrotor project that uses the specifications of the Crazy Flie and gives a simulation of it in an outdoor environment.

## State of the Art
In order to get a start point of how this models are build I read the following papers:

* Comprehensive Simulation of Quadrotor UAVs Using ROS and Gazebo *by Johannes Meyer, Alexander Sendobry, Stefan Kohlbrecher, Uwe Klingauf and Oskar von Stryk 2*

* Full Control of a Quadrotor *by Samir Bouabdallah and Roland Siegwart*

* Quadcopter Dynamics, Simulation, and Control

## Resources

* [Hector Quadrotor](http://wiki.ros.org/hector_quadrotor)  

* [Gazebo](http://gazebosim.org/)  

## Main Ideas

* Calculate a brushless motor thrust from its voltage

* PIDs working on a low level getting data from an onboard IMU and for stabilization

* Inertia of the drone taking part in its controllers

![hector_crazyflie_sim](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/hector_quad.png?raw=true)  


# 2 - Stabilization and Reference Follower (Still working on ...)

## Intro

On the previous section I mentioned that PID controllers at a low level take the role of stabilizing quadrotors. This is not entirelly true, although it can be efficient in stabilizing the rotational axes (using a [gyro](http://en.wikipedia.org/wiki/Gyroscope)) and its altitude (using a [barometer](http://www.seeedstudio.com/wiki/Grove_-_Barometer_Sensor)) they fail to stabilize any movement on the plane paralel to the floor, in other words, the quadrotor drifts like an air ballon. To solve this problem an external reference is required, for example, some outdoor drones uses a GPS or the AR parrot like I mentioned before uses a camera looking to the ground.  

My starting objective is to stabilize my AR Parrot using an outside reference (I'm going to start with tags). The starting idea is to use controllers, to perform the following functions:

## Altitude Control
![schematic1](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/schematic1.PNG?raw=true)  

A PID controller that keeps the drone altitude the same of the rerential (H meters in blue).


## Alignment Control
![schematic2](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/schematic2.PNG?raw=true) 

A PID controller that keeps the drone allign with the rerential (Red arrows).

## Distance Control
A PID controller that keeps the drone in a constant distance from the referential.


## Rotation Control
![schematic3](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/schematic3.PNG?raw=true)

A controller able to yaw and move the drone so it is always facing the referential.


## Resources

* [AR Drone Tutorials](https://github.com/mikehamer/ardrone_tutorials)
