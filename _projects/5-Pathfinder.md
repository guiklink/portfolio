---
layout: project
title: Pathfinder with A*
repository: https://github.com/guiklink/ME449_Astar
date: February 10, 2015
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/A_star/logo.png?raw=true
---

# Page On Construction {#index-ignore}

<article></article><br/>



# Introduction

Exploration robots are able move in complex environtments using its censors to detect and avoid obstacles and find good paths towards its goal. For this matter, during my Robotic Manipulation class we studied different kinds of pathfinder alghorithms. Being A* the most popular of them, our final assignment consisted on a Python implementation of it aiming to find the best route in a graph, composed by nodes where a robot could follow in a straight line without a collision. In addition in order to test my A* alghotithm, I also implemented a dynamic version of it using the Pygame library to build a dynamic interface so the alghoritm's execution can be seen in real time.  

# Real Time A*
The real time A* is not only a neat way to see the execution of the A* module but also a good way to see if it is working the way I desired. As said before this interface uses the Pygame library and in order to make it work a start point and end point (obstacles are optional) need to be input. The box bellow shows the keys instructions:

<iframe src="https://player.vimeo.com/video/137770025" width="500" height="521" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/137770025">Astar</a> from <a href="https://vimeo.com/user43396191">Guilherme Klink</a> on <a href="https://vimeo.com">Vimeo</a>.</p>


![Live_aStar_Legend](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/A_star/Live_aStar_Legend.png?raw=true)

# Graph Path Finder
For this approach, given a graph composed by an **N** number of nodes where a route is a straight line between each node, the alghorithm will try to compute the sequence of routes it will take a starting node to an end node. Although, it will not consider rotes that goes through or collide with an obstacle (the robot will have a radius).

The following steps will be taken on when the path finder script is executed:

![Image of a legend](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/A_star/Legend_1.png?raw=true)

## Step 1: Scenario Overview
Displays the location of all nodes, start, goal and obstacles.   

![step1](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/A_star/step1.png?raw=true)

## Step 2: All Routes
All routes are displayed even if not feasible. Once again, a route is a straight line between two nodes.   

![step2](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/A_star/step2.png?raw=true)

## Step 3: Eliminate Invalid Nodes 
All nodes inside an obstacle are desconsidered.   

![step3](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/A_star/step3.png?raw=true)

## Step 4: Eliminate invalid routes
The alghorithm calculates wich route if taken (considering the robot radius) will colide to an object and eliminate it.   

![step4](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/A_star/step4.png?raw=true)

## Step 5: Best Path
The A* module is used and the best path is highlighted in red.   

![step5](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/A_star/step5.png?raw=true)

