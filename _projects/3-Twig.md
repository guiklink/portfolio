---
layout: project
title: Twig Walking Animation
date: August 31, 2015
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/Twig_logo.png?raw=true
---

### [Repository](https://github.com/guiklink/Unity3d_Twig)

<iframe src="https://player.vimeo.com/video/137813432" width="758" height="426" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/137813432">Simple walking GAIT</a> from <a href="https://vimeo.com/user43396191">Guilherme Klink</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

# Introduction
The game industry has been showing one of the fastest grows among all other sectors of the economy and estimated sales in 2015 will reach $111.1 billion dollars. With such accreditation and crescent competition among its ranks is no surprise that companies like Bungie invested $150 million dollars on its later game "Destiny" or Konami invested over $60 million dollars in its coming "Metal Gear Solid: Phantom Pain". Aiming to top their sales and reach more and more players, game producers found out that the secret to make a best selling game is it to make it immersive. Modern games improve the player experience by combining the narrative aspect of the movies and interactivity. In order to keep this immersion,  a key factor is realism, the player's experience can be instantly ravaged if realism is broken.   

What is realism in something fictional?   

The answer is simple, in games things need to behave, look and move according to what would be expected in the game "world". The game industry hires the best AI professionals to ensure that things in the game behave smartly and as expected, in fact lots of learning algorithms comes from games. The Final Fantasy franchise invested lots of resources in developing hair that looks so real that later was even incorporated in movies. The actor Kiefer Sutherland was hired to have his face motion captured just for the sake of perfectly  animating the face of the character Snake in Metal Gear using a technology similar to Hollywood Blockbusters.  

In order to understand the purpose of Twig, let's talk about the last aspect of the previous paragraph, the animation. To make things move appropriately , animators design various sets of animations for every specific movement they expect the character to perform from running, jumping and fighting to even plucking a flower from the ground. A character will have different animations to sit in different chairs, just because the chair's size changes. At this point you probably already guessed, generating this huge animation library is very expensive and time taking, not to mention prohibitive for universities and independent developers.  

What if instead of animating everything, we could use simplified a movement [GAIT](https://en.wikipedia.org/wiki/Gait)? Simple enough that it would not be to hard to implement nor require heavy processing power therefore not compromising other aspects of the game, but still able to adapt to small peculiarities and lower cost to produce. Twig was developed for these purposes by Professor Ian Douglas Horswill from Northwestern University.  

My project consisted of creating a Twig walking GAIT for a biped humanoid. As shown in the video, the Twig GAIT implemented, although simple, would do a very good job as the GAIT of robot (I couldn't stop thinking about C3PO), a cartoon character or maybe a kid.

# Unity3D
To implement the Twig walking GAIT I used [Unity3D](http://unity3d.com/5?gclid=CNngyI300scCFYI7gQodzBYMlQ), for the following reasons:

1. It can be used in both Windows and MAC
2. Produce games for all the most popular platforms and cellphones
3. It is available for free personal use
4. It supports C#, making most of developer tools from Microsoft available
5. It has a big and active community

# Outline

## Step 1: Getting a Ragdoll
A Twig kinematic character is composed by rigid rods (black lines) connected to each other by nodes (red dots) as shown in the figure bellow.

![Twig_Character](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/Twig%20doll.png?raw=true)

The Unity3D analogy for that would be the 3D ragdoll, a  character represented by rigid bodies fully affected by physics and connected by configurable joints. In principle, any [3D humanoid rigged character](http://docs.unity3d.com/Manual/Preparingacharacterfromscratch.html) can be transformed in a ragdoll through the [Ragdoll Wizard](http://docs.unity3d.com/Manual/wizard-RagdollWizard.html). 
**PS:** It is recommended that the rigged body is saved in a "T" like position. 

## Step 2: Configuring Colliders and Ragdoll Joints
The Ragdoll Wizard does a very good job for you in setting the raw ragdoll, however from my experience, if you want your character to be working properly and avoid having to redo work I strongly encourage you to spend some time doing a more detailed tuning on your character. 

### 2.1: Colliders
The Unity physics engine uses the [colliders boxes](http://docs.unity3d.com/Manual/class-BoxCollider.html) to calculate collisions and you can use them in your scripts to trigger events. By default the Ragdoll Wizard usually makes the colliders way bigger than what you really need (see raw the colliders in green on the figure bellow), what makes the ragdoll to move in a clumsy and sometime even unexpected ways. For my character I made sure they were almost overlapping with  the real size of each respective rigid body. 

![Colliders](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/colliders.png?raw=true)

### 2.2: Joints
Once again Unity does not do everything automatically for us. The ragdoll uses the [Character Joint](http://docs.unity3d.com/Manual/class-CharacterJoint.html) component that gives you three preconfigured axes of rotations. Make sure to put the orange gizmos as the axis of rotations for places that do not rotate back like knees and elbows, this is the only axis you can have different values for maximum and minimum rotation, while the others axis will rotate the same amount of degrees for both sides (if you configure with 40 it will rotate from -40 to +40). Finally, make sure each joint is rotating as much as you will need on each axis, usually the arms require the most attention. In the figure bellow I show an example of how the joints in the arm should be set.

![Joints](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/JointsConfig.png?raw=true)

### 2.3: Add the Foot Rigid Body
By default the Ragdoll wizard does not add a [rigid body](http://docs.unity3d.com/Manual/class-Rigidbody.html) for the foot. In my implementation of Twig the forces that move the leg are added in the foot, the upper leg controller's only function is to lock undesired rotations so the character does not move in undesired ways. Therefore, I added a rigid body for the each foot and attached it to its respective leg through a [Character Joint](http://docs.unity3d.com/Manual/class-CharacterJoint.html). 

## Step 3: Making the Ragdoll Stand
Making the ragdoll stand was perhaps the most important step, since it needs to stand stable enough so it does not make the walking GAIT weird and too hard to implement. For this purpose a PID controller calculates the force necessary to maintain the body in a reference point. This controller is added in both the hips and head of the doll and maintains the center of mass of the respective body in a 3D reference (X, Y, Z) where Y is the height from the floor and X and Z is the midpoint of the feet in the XZ-plane (see the figure below).

![Mid Point](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/midpoint.png?raw=true)

One of the advantages of this implementation is that the ragdoll can easily crouch by reducing the referential value of Y. This controller was implemented in the script *StandSwat*.  
 
Configuring the gains of the controller was challenging, as I said before getting a stable position was crucial for the walking, after I got a raw tuning of the proportional gain, I tunned the other PID constants by throwing different weights on my ragdoll and looking at its response, as shown in the video bellow.

<iframe src="https://player.vimeo.com/video/137815980" width="500" height="281" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/137815980">Testing PID gains</a> from <a href="https://vimeo.com/user43396191">Guilherme Klink</a> on <a href="https://vimeo.com">Vimeo</a>.</p>
 
In the video the constants are almost tuned still with some damping and integral needing to be added. In addition to standing, the script *StandSwat* is also responsible for rotating the body according to the referential given by the state machine.

## Step 4: The State Machine
The main difference of my approach of Twig in comparison to the one proposed by Professor Horswill is the use of a state machine to coordinate each actuator in the characters body. The most challenging part of this project and the main cause of bugs and misbehavior, is the fact that so many controllers works simultaneously to keep the GAIT compliant. In my opinion, using a state machine to manage when each part body will move and rotate makes implementing and debugging much easier. 

The diagram below shows the schematic of the state transition:

![State Diagram](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/statemachinediagram.png?raw=true)

**Stand:** at this state the ragdoll stays idle and the joints of the leg lock in the respective position.  
**Crouch to Walk:** here the ragdoll crouches by an arbitrary amount so one of its legs have enough length to reach for a step.   
**Pick Leg:** the positions of the legs are calculated with respect to the hips coordinate frame, the leg that is behind will be picked as the one to step.   
**Calculate Left/Right Step:** Using the maximum length of the leg, the position of both feet and the position of the hips, an algorithm calculates where the respective step should end.   
**Left/Right Step:** A force moves the respective foot to the position calculated in the previous state.   
**Rise to Stand:** When the system stops getting an input to walk, the next step will be made so both legs are aligned, then the ragdoll will raise its hips to the same height from where it started moving.  

## Step 5: Moving the Arms
During walking the movement of the arms is fairly simple. Using the same state machine of the foot a discrete pre-configured inpulse is added on the oposite direction that the force is added on the feet. For example, at the state "Left Step" an inpulse is added on the right arm to the front and on the left arm to the back.

# Next Steps

* Develop a running GAIT
* Make two ragdolls play tag