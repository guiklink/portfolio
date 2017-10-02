---
layout: project
title: Twig Walking Animation
repository: https://github.com/guiklink/Unity3d_Twig
date: August 31, 2015
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/Twig_logo.png?raw=true
---

<article></article><br/>

<iframe src="https://player.vimeo.com/video/137813432" width="1137" height="639" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/137813432">Simple walking GAIT</a> from <a href="https://vimeo.com/user43396191">Guilherme Klink</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

# Introduction
Aiming to top their sales and reach more players, video game producers found out that the secret to make a best selling game is it to make it realistic.  

However, what is realism in something fictional such a game?    

The answer is simple, in games things need to behave, look and move accordingly to what would be expected in the "world" where the game takes place. In order to understand the purpose of Twig, let's talk about how things move accordingly. Animators design various sets of animations for every specific movement they expect the character to perform from running, jumping and fighting to even plucking a flower from the ground. A character will have different animations to sit in different chairs, just because the chair's size changes. At this point you probably already guessed, generating this huge animation library is very expensive and time taking, not to mention prohibitive for universities and independent developers.  

The idea behind TWIG is to create a simplified movement [gait](https://en.wikipedia.org/wiki/Gait), simple enough that it would not be too hard to implement nor require heavy processing power, therefore not compromising other aspects of the game (e.g. AI and rendering), but still able to adapt to small peculiarities and with a lower cost to produce. Twig was developed by Professor Ian Douglas Horswill from Northwestern University.  

My project consisted of creating a Twig walking GAIT for a biped humanoid. As shown in the video, the Twig GAIT implemented, although simple, would do a very good job as the GAIT of robot (I couldn't stop thinking about C3PO), a cartoon character or maybe a kid.

# Unity3D
My implementation of Twig used Unity3D. [Unity3D](http://unity3d.com/5?gclid=CNngyI300scCFYI7gQodzBYMlQ) is a common simulation and graphics rendering engine used in generating virtual environments. Some benefits of Unity3D are as follows:

1. It can be used in both Windows and MAC
2. Produce games for all the most popular platforms and cellphones
3. It is available for free personal use
4. It supports C#, making most of developer tools from Microsoft available
5. It has a big and active community

## Getting a Ragdoll
A Twig kinematic character is composed by rigid rods (black lines) connected to each other by nodes (red dots) as shown in the figure bellow.

![Twig_Character](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/Twig%20doll.png?raw=true)

The Unity3D analogy for that would be the 3D ragdoll, a  character represented by rigid bodies fully affected by physics and connected by configurable joints. In principle, any [3D humanoid rigged character](http://docs.unity3d.com/Manual/Preparingacharacterfromscratch.html) can be transformed in a ragdoll through the [Ragdoll Wizard](http://docs.unity3d.com/Manual/wizard-RagdollWizard.html). 
**PS:** It is recommended that the rigged body is saved in a "T" like position. 

## Configuring Colliders and Ragdoll Joints
The Ragdoll Wizard does a very good job for you in setting the raw ragdoll, however from my experience, if you want your character to be working properly and avoid having to redo work I strongly encourage you to spend some time doing a more detailed tuning on your character. FOr a more detailled description on how I have configured my ragdoll click [here](https://github.com/guiklink/portfolio/blob/gh-pages/public/Documents/twig/ragdoll_config.md).

## Making the Ragdoll Stand
Before being able to walk the ragdoll needs to be able to stand. This was perhaps the most important step, since it needs to stand stable and general enough so it do not complicate the implementation of other movements. For standing, PID controller applies force both in the hips and head, in order to maintain the center of mass of the body alligned according to a computated reference point. 

### Computing the Reference Point {#index-ignore}
![Mid Point](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/midpoint.png?raw=true)

The figure above exmplifies how the reference point for the hips is calculated. Having a 3D coordinates dictaded by the world-frame at the top right, Y is an arbitrary heigh from the floor and X and Z is the mid-point between the feet. There are two big advantages of calculating the reference point this way. First, by simply decreasing the value of Y you can easily make the ragdoll crouch, or by increasing and decreasing in a sequence you can make the ragdoll jump. Second, since X and Z is always updated as the mid-point of the feet, when one of the foot moves forward the mid-point will also move forward (the value of Z changes), thus the force that makes the upper body moves when walking will be automatically generated by the PID trying to allign to the new reference.
 
### PID Gains
Configuring the gains of the controller was challenging, as I said before getting a stable position was crucial for the walking. Too big gains makes the ragdoll to move unrealistic fast or keep contantly shaking, while to small gains would not be enought to rise the doll in a appropriate standing pose. After I got a raw tuning of the proportional gain, I tunned the other PID constants by throwing different weights on my ragdoll and looking at its response, as shown in the video bellow.

<iframe src="https://player.vimeo.com/video/137815980" width="750" height="423" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/137815980">Testing PID gains</a> from <a href="https://vimeo.com/user43396191">Guilherme Klink</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

## The State Machine For Walking
The ragdoll has over fifteen rigid bodies and more than double number of joints, keep tracking of what it of this parts will be doing during a walking is not an ordinary task, many bugs were conceived by actuators or scripts not synchronized appropriately. Moreover, in the future my ragdoll will have more gaits other than walking (e.g. running, kicking, seating). To manage how all body parts are actuated, which movement will be performed and facilitate debugging I used a state machine script, perhaps the biggest difference of my approach of Twig in comparison to the one original work from Professor Horswill.

The diagram below shows the schematic of the state transition:

![State Diagram](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/statemachinediagram.png?raw=true)

**Stand:** at this state the ragdoll stays idle and the joints of the leg lock in the respective position.  
**Crouch to Walk:** here the ragdoll crouches by an arbitrary amount so one of its legs have enough length to reach for a step.   
**Pick Leg:** the positions of the legs are calculated with respect to the hips coordinate frame, the leg that is behind will be picked as the one to step.   
**Calculate Left/Right Step:** Using the maximum length of the leg, the position of both feet and the position of the hips, an algorithm calculates where the respective step should end.   
**Left/Right Step:** A force moves the respective foot to the position calculated in the previous state.   
**Rise to Stand:** When the system stops getting an input to walk, the next step will be made so both legs are aligned, then the ragdoll will raise its hips to the same height from where it started moving.  

## Moving the Arms
Moving the arms for walking in theory is very simple, a torque is added on the shoulder joint and elbow joint as soon as the opposite step is given. Still, a lots of constants needs to be properly calibrated, the initial amount of torque needs to be adjusted according to different types of ragdolls and different walking speeds.  
A mathematical function actuating as a spring damping gives the arm a more natural oscillating behavior. The amount and damping frequency of this function also can be customized to different ragdoll models. The figures bellow shown an schematic of the right arm swing.

### Right Arm Swing {#index-ignore}
![arm_swing_front](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/arm_swinging_front.png?raw=true)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![arm_swing_back](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/arm_swinging_back.png?raw=true)

# Next Steps

### Running Gait {#index-ignore}
For a running gait the the body will have a forward inclination and both arms and legs will move faster.

### Realistic Turning {#index-ignore}
Right now I am not satisfied with the feet are posiotining with tunning. On the original twig implementation the dolls used had legs but no feet, therefore turning was implemented by rotating the hips and have the legs following it. For more realistic dolls this function is more challenging, since little steps to get the proper allignment of the foot will be necessary in order to perform an aceptable turning. 

### Two Ragdolls Playing Tag {#index-ignore}
With a proper turning and a running gait implemented I want to make two ragdools chase each other.

