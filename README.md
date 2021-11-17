# PROJ2-code
Cloudy with a Chance of Misclassification: An Exploration of Methods on Arctic Satellite Imagery

Overview:

The goal of this project was explore data through a number of classification methods, and ultimately build a model designed to detect clouds in polar regions from MISR data provided by the NASA satellite Terra. This project consisted of four main phases: the data exploration phase, preparation phase, modeling phase, and diagnostics phase. We implemented this process on the MISR data of three images offered by the paper "Daytime Arctic Cloud Detection Based on Multi-Angle Satellite Data with Case Studies". In this file will be provided an in depth description of how to reproduce our project in its entirety. 

Phase 1: Data Exploration

To begin our analysis of the data at hand we first simply calculated the percentage of data points that fell into each of the three classifications (cloudy, not-cloudy, and unlabled) for the three images. From there we created ggpairs plots on the first 2000 data points of the three images, using all but the x and y coordinate features. We proceeded to the creation of geom_tile plots for the three images that mapped the data points by x and y coordinate and colored them by classification. To gain better understanding of the correlation we sampled 10,000 data points from each image and used these samples to create corr.plots. Next we produced eight geom_boxplots that described each of the following features (NDAI, SD, CORR, DF, CF, BF, AF, AN) individually by label. This concluded our Data Exploration Phase.

Phase 2: Preparation

The initial 
