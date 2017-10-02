---
layout: project
title: Flying Robots
repository:
date: September 13, 2010
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/pj2_logo_small.JPG?raw=true
---

<article></article><br/>

# Introduction
As a roboticist I tend to believe in a future where robots and humans will be so connected that robots will be as present in dailly life as the smartphone that is always conveniently around. In my opinion, such connection will be conceived by the adaptability of robots into doing different tasks in harmony with us facilitating our simple daily tasks. This page is about my work with my favorite type of robot, the flying drone and it will be updated as I keep implementing new functionalities to it. I will start with the very basic and try to build it up in something useful (that I will probably be trying to use in my apartment). At this point, I do not expect drones to be helping me to carry burdensome loads of weight, the energy and aerodynamics for that is not easily available, or grasping complicated objects, after all that could be years of someone's PhD thesis in robotic manipulation. Instead, what if I could use some Control, Artificial Intelligence and Computer Vision techniques to transform my UAV into something like this:

![navi_following](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/navi_following.jpg?raw=true) ![navi_friend](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/navi_friend.jpg?raw=true)  
![navi_targeting](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/navi_targeting.jpg?raw=true) ![navi_hint](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/navi_hint.jpg?raw=true)

In the series "The Legend of Zelda: Ocarina of Time" for N64 (Nintendo - 1998), the fairy Navi assisted the protagonist Link on his quest to save a forsaken land by following him around, giving another view of perspective, recognizing friends and foes, conquering treacherous dungeons or even giving useful advices.

Now, what if we could bring that to the real world? Let's change the fairy wings for rotors and we have a quadrotor, that could follow us around, stay in our house or office and assist us with our daily quests (which diffently than Link I hope is not vanquishing goblins and shadow creatures) without really physicall. What if my fairy based drone could monitor my apartment, recognizing the people inside, film me spiking a volleyball from a nice angle, carry a letter to a co-worker whose desk is across the hall or even access the SIRI service from my iPhone and through a small on board speaker reminds me from my daily agenda (after seeing me goofing off on the couch). This kind of companion kind of robot  is my main motivation in working with UAV's. 
 
Achieving this idea is a long run and for that purpose I will try to follow a roadmap of small projects (that will probably go on even after my masters program) covering each specific area of knowledge that in my opinion will help me in building a final product.    

## My Flying Robots
These are the quadrotors that I am most familiar with (both have good documentation and active community online) and therefore will be used on my work.  

### [Crazy Flie](http://www.bitcraze.se/crazyflie/) {#index-ignore}
![Crazy_Flie](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/crazy_flie.JPG?raw=true)  
<br/>
Weighing less than 20g and totally open source this nano quadrotor is a great startup for any quadrotor aficionado. Its parts are very cheap and it is so light that is harmless if collides to something.  

### [AR Parrot 2.0](http://ardrone2.parrot.com/) {#index-ignore}
![AR_parrot](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/ar_parrot.JPG?raw=true)  
<br/>
Due to its popularity, well documented SDK and lots of [ROS](http://www.ros.org/) packages available, I would say the that AR Parrot series is a good choice for those who wants to have a HD camera on board transmitting through wifi without having to build their own quadrotor. In addition, the downwards camera provides an amazing stability algorithm in ambients with good illumination, what is perfect for indoor applications.

### [Cheap N' Tough Copter](http://guiklink.github.io/portfolio/projects/8-The_Cheap_n_Tought_Copter/) {#index-ignore}
![Cheap_N_Tough](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/cheap_hellicopter.jpg?raw=true)  
<br/>
Currently I am working on creating the Cheap N' Tough, a hacked toy hellicopter with a better IMU, radio comunication and my own controllers. 

# (Part 1) Dynamics and Simulation
When customizing quadrotors, by adding gadgets, increasing motors, changing its body shape or implementing functionalities, it is important to have a reliable simulation environment in order to try to predict the flaws (no one wants to break the dear drone). On this first project my goal was to understand the physics of quadrotors and how that is applied in the ROS simulator "Hector Quadrotor", so it can be altered and used to simulate different kinds of flying robots. 
The knowledge gathered from this project were key to implement my [2D Quadrotor Optimization](http://guiklink.github.io/portfolio/projects/2-2D_Quad/).

## State of the Art {#index-ignore}
Bellow the papers that I used on this task.

* Comprehensive Simulation of Quadrotor UAVs Using ROS and Gazebo *by Johannes Meyer, Alexander Sendobry, Stefan Kohlbrecher, Uwe Klingauf and Oskar von Stryk 2*

* Full Control of a Quadrotor *by Samir Bouabdallah and Roland Siegwart*

* Quadcopter Dynamics, Simulation, and Control

## Resources {#index-ignore}

* [Hector Quadrotor](http://wiki.ros.org/hector_quadrotor)  

* [Gazebo](http://gazebosim.org/)  

## Main Ideas {#index-ignore}

* Calculate a brushless motor thrust from its voltage

* PIDs working on a low level getting data from an onboard IMU and for stabilization

* Inertia of the drone taking part in its controllers

![hector_crazyflie_sim](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/hector_quad.png?raw=true)  


# (Part 2) Stabilization and Reference Follower (Still working on ...)

## Introduction {#index-ignore}

On the previous section I mentioned that PID controllers at a low level take the role of stabilizing quadrotors. This is not entirelly true, although it can be efficient in stabilizing the rotational axes (using a [gyro](http://en.wikipedia.org/wiki/Gyroscope)) and its altitude (using a [barometer](http://www.seeedstudio.com/wiki/Grove_-_Barometer_Sensor)) they fail to stabilize any movement on the plane paralel to the floor, in other words, the quadrotor drifts like an air ballon. To solve this problem an external reference is required, for example, some outdoor drones uses a GPS or the AR parrot like I mentioned before uses a camera looking to the ground.  

My starting objective is to stabilize my AR Parrot using an outside reference (I'm going to start with tags). The starting idea is to use controllers, to perform the following functions:

## Altitude Control {#index-ignore}
![schematic1](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/schematic1.PNG?raw=true)  

A PID controller that keeps the drone altitude the same of the rerential (H meters in blue).


## Alignment Control {#index-ignore}
![schematic2](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/schematic2.PNG?raw=true) 

A PID controller that keeps the drone allign with the rerential (Red arrows).

## Distance Control {#index-ignore}
A PID controller that keeps the drone in a constant distance from the referential.


## Rotation Control {#index-ignore}
![schematic3](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/flying_robots/schematic3.PNG?raw=true)

A controller able to yaw and move the drone so it is always facing the referential.


## Resources {#index-ignore}

* [AR Drone Tutorials](http://robohub.org/up-and-flying-with-the-ar-drone-and-ros-getting-started/)
