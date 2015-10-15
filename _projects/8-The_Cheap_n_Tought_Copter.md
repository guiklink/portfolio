---
layout: project
title: The Cheap N' Though Copter
repository: 
date: September 1, 2015
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/cheap_N_tought_copter/logo.jpg?raw=true
---

# On construction {#index-ignore}


<article></article><br/>



# Introduction

My goal in this project is an improvement for toy helicopters by adding a better IMU, controllers and communication, the Cheap N' Tought Copter. Taking advantage of the low price of producing this helicopters and its robustness I can have lots of drones for testing flying applications and many units to try Swarm algorithms. 


# Materials

### A Toy Hellicopter {#index-ignore}

![toy_hellicopter]()

For the scope of this project designing parts, 3D printing gears and picking motors can be a cumbersome and  unnecessary task, especially when you can buy a cheap [toy helicopters](http://www.amazon.com/gp/product/B00DPK11ZA/ref=s9_simh_gw_p21_d29_i4?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-1&pf_rd_r=0JV5BKHGHKT6ZDM0V93Y&pf_rd_t=36701&pf_rd_p=2079475242&pf_rd_i=desktop) to start from.

### PIC32

![PIC32MX250F128B]()

A 32 bit microcontroller from microchip the [PIC32](https://en.wikipedia.org/wiki/PIC_microcontroller) is small, cheap, light and used in a variety of applications. It was my choice as the onboard processor of my Cheap N' Though.

### IMU {#index-ignore}

![IMU]()

In order to achieve good controls and a stabilized flight a better IMU is required. For this function I chose the [LSM6DS3](https://www.sparkfun.com/products/13339) from Sparkfun. This device provides me data in 6 [DOF](https://en.wikipedia.org/wiki/Degrees_of_freedom_(mechanics)) and in addition, Sparkfun provides a good [code samples](https://github.com/sparkfun/SparkFun_LSM6DS3_Arduino_Library) of how to use it.

### Radio Comunicator {#index-ignore}

![radio_nRF24L01]()

A [radio breakout board](https://www.sparkfun.com/products/691) will be added so I can stablish a comunnication between the hellicopter and my computer. For this application I chose a radio that uses the nRF24L01 protocol, so I can communicate with the same antenna I have been using for my [Crazy Flies](https://www.bitcraze.io/crazyflie/).

### The NU32 {#index-ignore}

![NU32 / Nscope]()

The [NU32](http://hades.mech.northwestern.edu/index.php/NU32:_Introduction_to_the_PIC32) is a development board created at Northwestern University that makes prototyping PIC32 easier. With the [Nscope](http://nscope.org/) attached I also have a portable oscilloscope. I used the NU32 board to attach the two periferals from Spark Fun notted above and test them, once I converted the Arduino C++ (.ino) samples provided into standard C (.c) PIC32 code.

### My Prototype Board {#index-ignore}

![protoBoard]()

I protoboard mounted by myself with a standard PIC32MX250F128B and a LCD screen for a final test of the comunications before designing the final PCB.





