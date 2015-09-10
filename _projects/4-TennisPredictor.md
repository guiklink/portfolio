---
layout: project
title: Tennis Prediction App
date: February 10, 2015
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/tennis_predictor/logo.jpg?raw=true
---

[Project Website](http://guiklink.github.io/Tennis_Predictor_App/)  
[Repository](https://github.com/guiklink/Tennis_Predictor_App)

# On Construction  

# Introduction
During my Machine Learning class we learned the pros and cons of various types of alghoriths focused in taking automatically conclusions from datasets, automatize decisions and optimize resolutions for problems. As final project, due to our personal interest and lack of prediction apps for tennis, me and my partner decided to use decision trees to perform predictions regard matches.  

## The App
The app was developed in [Python](https://www.python.org/), using [TkInter](https://wiki.python.org/moin/TkInter) for graphical interface, [Scikit](http://scikit-learn.org/stable/) as the module for learning the decision trees and SQL (see **Data**) for data manipulation.
I executable was created for Linux, although a executable for windows still needs to be generated (see the [website](http://guiklink.github.io/Tennis_Predictor_App/) for instructions of how to build the app from source).  

## Data
Most of the time invested in this project was undoubtedly towards having a good amount of data in good quality and properly formatted so it could be processed using SciKit. A lot of typos, blanks and misleading data needed to be correct, not to mention lack of information for some of the years that needed to be deduced by crossing information of different datasets. In order to make the management of such massive amount of data possible, the raw data was imported to a [SQLite](https://www.sqlite.org/) database using a [python script](https://github.com/guiklink/Tennis_Predictor_App/blob/master/Python/importFiles.py) and SQL queries were aplied to create the master table **ALL_TOURNAMENTS_2011_2015**.   

### Data sources

* [Heavy Topspin](http://heavytopspin.com/2013/11/26/the-match-charting-project/)
* [Jeff's Sackmann match charting project](https://github.com/JeffSackmann/tennis_MatchChartingProject)
* [Jeff's Sackmann point by point](https://github.com/JeffSackmann/tennis_slam_pointbypoint)
* [Jeff's Sackmann matches](https://github.com/JeffSackmann/tennis_atp)
* [ATP website](http://www.atpworldtour.com/)
* [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Tennis+Major+Tournament+Match+Statistics)
* [Tennis-Data.co.uk](http://www.tennis-data.co.uk/alldata.php)

## For more information check the project [website](http://guiklink.github.io/Tennis_Predictor_App/).

