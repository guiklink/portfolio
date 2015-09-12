---
layout: project
title: Get started with ROSJava
date: February 10, 2015
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/ROS_Java/ROS_JAVA.png?raw=true
---

<article></article>

#[Repository](https://github.com/guiklink/ME495_Rosjava_Startup) {#repository}

#Introduction 
This is a tutorial of how to get started with ROS Java by getting the [turtlesim](http://wiki.ros.org/turtlesim) walking in a square using [Java](https://en.wikipedia.org/wiki/Java_(programming_language)).   
 <br/>

##Installing rosjava
First install the source by following the steps [here](http://wiki.ros.org/rosjava/Tutorials/indigo/Source%20Installation). After installation if your not able to *catkin make* your workspace make sure you update all the packages (run a *'sudo apt-get update'* on a terminal). All the basic documentation about ROS Java, like how to create packages, messages and building libraries are documented [here](http://wiki.ros.org/rosjava) (you might have to change it for Indigo).   
<br/>

##Writing a Simple Publisher and Subscriber
A typical rosjava workspace looks slightly different than a ROS workspace, since it is not as tightly integrated with ROS as rospy or roscpp. Instead, rosjava uses [Gradle](http://www.gradle.org/) and [Maven](http://maven.apache.org/). The last is a build automation tool used primarily for Java projects. Maven addresses two aspects of building software: First, it describes how software is built, and second, it describes its dependencies. It also forces a lot of best practices and standards. Most of the java community uses Maven repository structure and standards as well as the dependency management. Gradle has a Maven plugin that adds support for deploying to Maven repositories. 
<br/>

##Creating Rosjava Packages {#crt-pakgs}
First, create a wokspace for your project. Then, create a [rosjava package](http://wiki.ros.org/rosjava_build_tools/Tutorials/hydro/Creating%20Rosjava%20Packages). Follow the directions upto step 5.1.Binary Projects (App) and then continue to the next step [Writing a Simple Publisher and Subscriber (Java)](http://wiki.ros.org/rosjava_build_tools/Tutorials/hydro/WritingPublisherSubscriber%28Java%29). [Note: Do not execute Step 5.2.Library Project. It seems to break catkin_make and compilation error follows anything you do in rosjava after that].
<br/>

### Repository Structure
When you run catkin_create_rosjava_pkg, rosjava create the repository slightly different to usual ros repository structures.


<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-cxkv{background-color:#ffffff}
.tg .tg-8xqh{font-weight:bold;background-color:#c0c0c0}
</style>
<table class="tg" style="undefined;table-layout: fixed; width: 449px">
<colgroup>
<col style="width: 213px">
<col style="width: 236px">
</colgroup>
  <tr>
    <th class="tg-8xqh">                     ROS</th>
    <th class="tg-8xqh">                     ROS Java</th>
  </tr>
  <tr>
    <td class="tg-cxkv">               Catkin Stack</td>
    <td class="tg-cxkv">Catkin Package/Gradle Multi-project</td>
  </tr>
  <tr>
    <td class="tg-cxkv">            Catkin Package</td>
    <td class="tg-cxkv">              Gradle Subproject</td>
  </tr>
</table>
<br>

<h2>Compiling with Gradle</h2>

When you run catkin_make, cMake runs through your entire workspace and when it gets to your new project, it will pass off the build to gradle.You could alternatively just compile your subproject alone with gradle (much faster than running catkin_make across your entire workspace):

<div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%">source devel<span style="color: #333333">/</span>setup<span style="color: #333333">.</span>bash
cd src<span style="color: #333333">/</span>rosjava_catkin_package_a<span style="color: #333333">/</span>my_pub_sub_tutorial
<span style="color: #333333">../</span>gradlew installApp
</pre></div>

<br>
<h1>Sending Messages for the Turtle</h1>

Now let's walk though on how to change the **Writing a Simple Publisher and Subscriber** subproject in order to publish messages for the [turtlesim](http://wiki.ros.org/turtlesim). Also, you can `git clone` the final package from [here](https://github.com/guiklink/ME495_Rosjava_Startup).

*NOTE:* Create a [Simple Publisher and Subscriber](http://wiki.ros.org/rosjava_build_tools/Tutorials/hydro/WritingPublisherSubscriber%28Java%29) subproject if you still have not yet.    

<br>
<h2>Adding Dependencies</h2>
First, we need to add the dependencies we are going to be using for this task. Remember that the [turtlesim](http://wiki.ros.org/turtlesim) subscribes to 'geometry_msgs.Twist' ROS message, therefore we must make sure that the compiler is aware of this to build our executables.
 
Go inside your **rosjava** package (**rosjava_catkin_package_a** if you strickly followed the tutorial above) and open the [CmakeList.txt](https://github.com/guiklink/ME495_Rosjava_Startup/blob/master/CMakeLists.txt) and [package.xml](https://github.com/guiklink/ME495_Rosjava_Startup/blob/master/package.xml).

<h3><em>CmakeList.txt</em></h3>
<div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%">##############################################################################
# CMake
##############################################################################

cmake_minimum_required(VERSION 2.8.3)
project(rosjava_catkin_package_a)

##############################################################################
# Catkin
##############################################################################

find_package(catkin REQUIRED rosjava_build_tools)
find_package(catkin REQUIRED COMPONENTS geometry_msgs)

# Set the gradle targets you want catkin&#39;s make to run by default, e.g.
#   catkin_rosjava_setup(installApp)
# Note that the catkin_create_rosjava_xxx scripts will usually automatically
# add tasks to this for you when you create subprojects.
catkin_rosjava_setup(installApp publishMavenJavaPublicationToMavenRepository)

catkin_package()

##############################################################################
# Installation
##############################################################################

# Change this to match the maven group name you have specified in the 
# allprojects closure the root build.gradle
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_MAVEN_DESTINATION}/com/github/rosjava/${PROJECT_NAME}/ 
        DESTINATION ${CATKIN_GLOBAL_MAVEN_DESTINATION}/com/github/rosjava/${PROJECT_NAME})
</pre></div>


* The code line  ```find_package(catkin REQUIRED COMPONENTS geometry_msgs)``` must be added to include [geometry_msgs](http://wiki.ros.org/geometry_msgs).

<br>
<h3><em>package.xml</em></h3>
<!-- HTML generated using hilite.me --><div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #557799">&lt;?xml version=&quot;1.0&quot;?&gt;</span>
<span style="color: #007700">&lt;package&gt;</span>
  <span style="color: #007700">&lt;name&gt;</span>rosjava_catkin_package_a<span style="color: #007700">&lt;/name&gt;</span>
  <span style="color: #007700">&lt;version&gt;</span>0.1.0<span style="color: #007700">&lt;/version&gt;</span>
  <span style="color: #007700">&lt;description&gt;</span>The rosjava_catkin_package_a package<span style="color: #007700">&lt;/description&gt;</span>

  <span style="color: #888888">&lt;!-- One maintainer tag required, multiple allowed, one person per tag --&gt;</span> 
  <span style="color: #888888">&lt;!-- Example:  --&gt;</span>
  <span style="color: #888888">&lt;!-- &lt;maintainer email=&quot;jane.doe@example.com&quot;&gt;Jane Doe&lt;/maintainer&gt; --&gt;</span>
  <span style="color: #007700">&lt;maintainer</span> <span style="color: #0000CC">email=</span><span style="background-color: #fff0f0">&quot;klink@todo.todo&quot;</span><span style="color: #007700">&gt;</span>klink<span style="color: #007700">&lt;/maintainer&gt;</span>


  <span style="color: #888888">&lt;!-- One license tag required, multiple allowed, one license per tag --&gt;</span>
  <span style="color: #888888">&lt;!-- Commonly used license strings: --&gt;</span>
  <span style="color: #888888">&lt;!--   BSD, MIT, Boost Software License, GPLv2, GPLv3, LGPLv2.1, LGPLv3 --&gt;</span>
  <span style="color: #007700">&lt;license&gt;</span>Apache 2.0<span style="color: #007700">&lt;/license&gt;</span>


  <span style="color: #888888">&lt;!-- Url tags are optional, but mutiple are allowed, one per tag --&gt;</span>
  <span style="color: #888888">&lt;!-- Optional attribute type can be: website, bugtracker, or repository --&gt;</span>
  <span style="color: #888888">&lt;!-- Example: --&gt;</span>
  <span style="color: #888888">&lt;!-- &lt;url type=&quot;website&quot;&gt;http://wiki.ros.org/rosjava_catkin_package_a&lt;/url&gt; --&gt;</span>


  <span style="color: #888888">&lt;!-- Author tags are optional, mutiple are allowed, one per tag --&gt;</span>
  <span style="color: #888888">&lt;!-- Authors do not have to be maintianers, but could be --&gt;</span>
  <span style="color: #888888">&lt;!-- Example: --&gt;</span>
  <span style="color: #888888">&lt;!-- &lt;author email=&quot;jane.doe@example.com&quot;&gt;Jane Doe&lt;/author&gt; --&gt;</span>


  <span style="color: #888888">&lt;!-- The *_depend tags are used to specify dependencies --&gt;</span>
  <span style="color: #888888">&lt;!-- Dependencies can be catkin packages or system dependencies --&gt;</span>
  <span style="color: #888888">&lt;!-- Examples: --&gt;</span>
  <span style="color: #888888">&lt;!-- Use build_depend for packages you need at compile time: --&gt;</span>
  <span style="color: #888888">&lt;!--   &lt;build_depend&gt;message_generation&lt;/build_depend&gt; --&gt;</span>
  <span style="color: #888888">&lt;!-- Use buildtool_depend for build tool packages: --&gt;</span>
  <span style="color: #888888">&lt;!--   &lt;buildtool_depend&gt;catkin&lt;/buildtool_depend&gt; --&gt;</span>
  <span style="color: #888888">&lt;!-- Use run_depend for packages you need at runtime: --&gt;</span>
  <span style="color: #888888">&lt;!--   &lt;run_depend&gt;message_runtime&lt;/run_depend&gt; --&gt;</span>
  <span style="color: #888888">&lt;!-- Use test_depend for packages you need only for testing: --&gt;</span>
  <span style="color: #888888">&lt;!--   &lt;test_depend&gt;gtest&lt;/test_depend&gt; --&gt;</span>
  <span style="color: #007700">&lt;buildtool_depend&gt;</span>catkin<span style="color: #007700">&lt;/buildtool_depend&gt;</span>
  <span style="color: #007700">&lt;build_depend&gt;</span>rosjava_build_tools<span style="color: #007700">&lt;/build_depend&gt;</span>
  <span style="color: #007700">&lt;build_depend&gt;</span>geometry_msgs<span style="color: #007700">&lt;/build_depend&gt;</span>
  <span style="color: #007700">&lt;run_depend&gt;</span>geometry_msgs<span style="color: #007700">&lt;/run_depend&gt;</span>


  <span style="color: #888888">&lt;!-- The export tag contains other, unspecified, tags --&gt;</span>
  <span style="color: #007700">&lt;export&gt;</span>
    <span style="color: #888888">&lt;!-- You can specify that this package is a metapackage here: --&gt;</span>
    <span style="color: #888888">&lt;!-- &lt;metapackage/&gt; --&gt;</span>

    <span style="color: #888888">&lt;!-- Other tools can request additional information be placed here --&gt;</span>

  <span style="color: #007700">&lt;/export&gt;</span>
<span style="color: #007700">&lt;/package&gt;</span>
</pre></div>

* The lines ```<build_depend>geometry_msgs</build_depend>``` and ```<run_depend>geometry_msgs</run_depend>``` to add [geometry_msgs](http://wiki.ros.org/geometry_msgs).

<!-- HTML generated using hilite.me --><div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #008800; font-weight: bold">import</span> <span style="color: #0e84b5; font-weight: bold">org.ros.concurrent.CancellableLoop</span><span style="color: #333333">;</span>
<span style="color: #008800; font-weight: bold">import</span> <span style="color: #0e84b5; font-weight: bold">org.ros.namespace.GraphName</span><span style="color: #333333">;</span>
<span style="color: #008800; font-weight: bold">import</span> <span style="color: #0e84b5; font-weight: bold">org.ros.node.AbstractNodeMain</span><span style="color: #333333">;</span>  <span style="color: #888888">// This library give us the AbstractNodeMain interface (see ahead)</span>
<span style="color: #008800; font-weight: bold">import</span> <span style="color: #0e84b5; font-weight: bold">org.ros.node.ConnectedNode</span><span style="color: #333333">;</span>
<span style="color: #008800; font-weight: bold">import</span> <span style="color: #0e84b5; font-weight: bold">org.ros.node.NodeMain</span><span style="color: #333333">;</span>          
<span style="color: #008800; font-weight: bold">import</span> <span style="color: #0e84b5; font-weight: bold">org.ros.node.topic.Publisher</span><span style="color: #333333">;</span>  <span style="color: #888888">// Import the publisher</span>
<span style="color: #008800; font-weight: bold">import</span> <span style="color: #0e84b5; font-weight: bold">geometry_msgs.Twist</span><span style="color: #333333">;</span>           <span style="color: #888888">// Import geometry_msgs.Twist ... remember to incluse this message into your dependencie files </span>
</pre></div>


Import a bunch of ROSjava classes that will be used in our code.   
[CancellableLoop](http://docs.rosjava.googlecode.com/hg/rosjava_core/html/javadoc/org/ros/concurrent/CancellableLoop.html) this is a loop that can be used similarly to the python command ```` while not rospy.is_shutdown():```.   
[AbstractNodeMain](http://docs.rosjava.googlecode.com/hg/rosjava_core/html/javadoc/org/ros/node/AbstractNodeMain.html) every node made in ROSJava must extend this abstract class in order to be viewed as a node.   
[ConnectedNode](http://docs.rosjava.googlecode.com/hg/rosjava_core/html/javadoc/org/ros/node/ConnectedNode.html) class for connected nodes (use explained ahead).   
[topic.Publisher](http://docs.rosjava.googlecode.com/hg/rosjava_core/html/javadoc/org/ros/node/topic/Publisher.html) makes the publish interface available to implement.    
[geometry_msgs.Twist](http://docs.ros.org/api/geometry_msgs/html/msg/Twist.html) import Twist messages.

<div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%">  <span style="color: #008800; font-weight: bold">public</span> <span style="color: #333399; font-weight: bold">void</span> <span style="color: #0066BB; font-weight: bold">onStart</span><span style="color: #333333">(</span><span style="color: #008800; font-weight: bold">final</span> ConnectedNode connectedNode<span style="color: #333333">)</span> <span style="color: #333333">{</span>
    <span style="color: #008800; font-weight: bold">final</span> Publisher<span style="color: #333333">&lt;</span>geometry_msgs<span style="color: #333333">.</span><span style="color: #0000CC">Twist</span><span style="color: #333333">&gt;</span> publisher <span style="color: #333333">=</span>
        connectedNode<span style="color: #333333">.</span><span style="color: #0000CC">newPublisher</span><span style="color: #333333">(</span><span style="background-color: #fff0f0">&quot;/turtle1/cmd_vel&quot;</span><span style="color: #333333">,</span> geometry_msgs<span style="color: #333333">.</span><span style="color: #0000CC">Twist</span><span style="color: #333333">.</span><span style="color: #0000CC">_TYPE</span><span style="color: #333333">);</span> <span style="color: #888888">// That&#39;s how you create a publisher in Java!</span>
</pre></div>


On running time ```ConnectedNode connectedNode``` gives the connection between your node and the ```roscore`` **master** running.
Afterwards, a **publisher** is created from the connection, where the topic and message type is defined respectively.

<div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%">    connectedNode<span style="color: #333333">.</span><span style="color: #0000CC">executeCancellableLoop</span><span style="color: #333333">(</span><span style="color: #008800; font-weight: bold">new</span> CancellableLoop<span style="color: #333333">()</span> <span style="color: #333333">{</span>
      <span style="color: #008800; font-weight: bold">private</span> <span style="color: #333399; font-weight: bold">int</span> sequenceNumber<span style="color: #333333">;</span>

      <span style="color: #555555; font-weight: bold">@Override</span>
      <span style="color: #008800; font-weight: bold">protected</span> <span style="color: #333399; font-weight: bold">void</span> <span style="color: #0066BB; font-weight: bold">setup</span><span style="color: #333333">()</span> <span style="color: #333333">{</span>
        sequenceNumber <span style="color: #333333">=</span> <span style="color: #0000DD; font-weight: bold">0</span><span style="color: #333333">;</span>
      <span style="color: #333333">}</span>
</pre></div>


On the first line an **CancellableLoop** class is passed to be executed in our executing node. And initialize a ```sequenceNumber``` variable to keep track of the times that the loop is exectued.

<div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #555555; font-weight: bold">@Override</span>
      <span style="color: #008800; font-weight: bold">protected</span> <span style="color: #333399; font-weight: bold">void</span> <span style="color: #0066BB; font-weight: bold">loop</span><span style="color: #333333">()</span> <span style="color: #008800; font-weight: bold">throws</span> InterruptedException <span style="color: #333333">{</span>
        geometry_msgs<span style="color: #333333">.</span><span style="color: #0000CC">Twist</span> twist <span style="color: #333333">=</span> publisher<span style="color: #333333">.</span><span style="color: #0000CC">newMessage</span><span style="color: #333333">();</span> <span style="color: #888888">// Init a msg variable that of the publisher type</span>
        sequenceNumber<span style="color: #333333">++;</span>

        <span style="color: #008800; font-weight: bold">if</span> <span style="color: #333333">(</span>sequenceNumber <span style="color: #333333">%</span> <span style="color: #0000DD; font-weight: bold">3</span> <span style="color: #333333">==</span> <span style="color: #0000DD; font-weight: bold">0</span><span style="color: #333333">)</span> <span style="color: #333333">{</span>          <span style="color: #888888">// Every 3 executions of the loop (aprox. 3*1000ms = 3 sec)</span>
          twist<span style="color: #333333">.</span><span style="color: #0000CC">getAngular</span><span style="color: #333333">().</span><span style="color: #0000CC">setZ</span><span style="color: #333333">(</span>Math<span style="color: #333333">.</span><span style="color: #0000CC">PI</span><span style="color: #333333">/</span><span style="color: #0000DD; font-weight: bold">2</span><span style="color: #333333">);</span>   <span style="color: #888888">// Steer the turtle left</span>
        <span style="color: #333333">}</span>
        <span style="color: #008800; font-weight: bold">else</span><span style="color: #333333">{</span>
          twist<span style="color: #333333">.</span><span style="color: #0000CC">getLinear</span><span style="color: #333333">().</span><span style="color: #0000CC">setX</span><span style="color: #333333">(</span><span style="color: #0000DD; font-weight: bold">2</span><span style="color: #333333">);</span>            <span style="color: #888888">// In the meantime keeps going foward </span>
        <span style="color: #333333">}</span>
        
        publisher<span style="color: #333333">.</span><span style="color: #0000CC">publish</span><span style="color: #333333">(</span>twist<span style="color: #333333">);</span>       <span style="color: #888888">// Publish the message (if running use rostopic list to see the message)</span>

        Thread<span style="color: #333333">.</span><span style="color: #0000CC">sleep</span><span style="color: #333333">(</span><span style="color: #0000DD; font-weight: bold">1000</span><span style="color: #333333">);</span>             <span style="color: #888888">// Sleep for 1000 ms = 1 sec</span>
      <span style="color: #333333">}</span>
    <span style="color: #333333">});</span>
  <span style="color: #333333">}</span>
<span style="color: #333333">}</span>
</pre></div>


**CancellableLoop** must ```have a loop()``` method, which is obviously the loop that will be executed until the node is killed or stopped. ''' if (sequenceNumber % 3 == 0)''' at every 3 executions of the loop a twist message with a rotation on the Z-axis of 90 degrees is created to make the robot steer to the left. In every other execution a forward velocity twist message is created.
Finally, ```publisher.publish(twist);``` published the message and the node sleeps for 1000 ms.

##Compiling the Code##
There are two ways to compile your code:
1. Go to your workspace root directory and do a ```catkin_make```
2. Sometimes doing a ```catkin_make``` can take ages! In this cases you can have **gradle** to compile your local package. In order to do this go to your sub project directory and execute ```../gradlew installApp```. Again, check the Writing a [Simple Publisher and Subscriber](http://wiki.ros.org/rosjava_build_tools/Tutorials/hydro/WritingPublisherSubscriber%28Java%29) tutorial for more information.

##You're ready to execute it...##
Execute a master ```roscore```, pop up the turtlesim ```rosrun turtlesim turtlesim_node``` and execute your brand new Java node to see your turtle draw squares!

###Remembering...###
To run your node go into:  
```> cd src/rosjava_catkin_package_a/my_pub_sub_tutorial/build/install/my_pub_sub_tutorial/bin```  
and use the following command  
```> ./my_pub_sub_tutorial com.github.rosjava_catkin_package_a.my_pub_sub_tutorial.Talker```
 

