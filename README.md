# PROJ2-code
Cloudy with a Chance of Misclassification: An Exploration of Methods on Arctic Satellite Imagery

Overview:

The goal of this project was explore data through a number of classification methods, and ultimately build a model designed to detect clouds in polar regions from MISR data provided by the NASA satellite Terra. This project consisted of four main phases: the data exploration phase, preparation phase, modeling phase, and diagnostics phase. We implemented this process on the MISR data of three images offered by the paper "Daytime Arctic Cloud Detection Based on Multi-Angle Satellite Data with Case Studies". In this file will be provided an in depth description of how to reproduce our project in its entirety. 

Phase 1: Data Exploration

To begin our analysis of the data at hand we first simply calculated the percentage of data points that fell into each of the three classifications (cloudy, not-cloudy, and unlabled) for the three images. From there we created ggpairs plots on the first 2000 data points of the three images, using all but the x and y coordinate features. We proceeded to the creation of geom_tile plots for the three images that mapped the data points by x and y coordinate and colored them by classification. To gain better understanding of the correlation we created subsamples of 10,000 data points from each image and used these samples to create corr.plots. Next we produced eight geom_boxplots that described each of the following features (NDAI, SD, CORR, DF, CF, BF, AF, AN) individually by label. This concluded our Data Exploration Phase.

Phase 2: Preparation

The initial step to our Preparation Phase was to split the data into training, validation, and test sets. The first split we tried was a 60/20/20 split for the training, validation, and testing sets respectively. To achieve this we sampled 60% of the data points from each image and allocated the points to our trainings sets, next we sampled 50% of the remaining data from each image (20% of the total) and allocated this data to our validation sets, and the remaining data for each image was allocated to our test sets. The next split we create was a spin of 60/20/20 split, where this time we sampled equal rates of each classification type. That is we grouped the classification types and sampled 60% of each classification type for the three images and allocated these points to our training sets, then sampled 20% from each classification type of each image, to create both our validation and test sets. For the remainder of the paper, code, and this document the first split mentioned will be referred to as the method one split, and the second split mentioned will be referred to as the method two split. Moving to the next step of our Preparation phase we assessed the accuracy of the trivial classifier, setting all labels equal to negative one for each image. Next we used the training data from the method two split, to perform PCA analysis. We first performed the analysis on the radiance features (DF, CF, BF, AF, and AN) and made two geom_point plots for the first/second and thrid/fourth PCA directions. We then repeated this process for the non-radiance features (NDAI, SD, and CORR). Continuing on, agian with the method two training data, we created thre geom_point scatterplots to further understand Labels across DF vs. SD, Labels across CORR vs. NDAI, and Labels across XCoord vs. YCoord. Finally, we wrote two functions, one for returning class accuracies, and one for cross validation implementation accomplished by splitting the data equally into k folds, iterating through each fold, and outputting the accuracy of each fold.  

Phase 3: Modeling

We decided on four models to test: logistic regression, KNN, SVM, and a random forest. Total, we ran eight models, as each model was run with the method one and method two data splits. In this portion of the exploration we elected to only observe two classifiers (cloudy and not-cloudy), so we removed all unlabed points from our data. Additionally, we combined our method one trainng and validation data into one set, as well as our method two training and validation data. We used the trainControl function to set up our cross validation, and elected to use five folds. In the trainControl function we set savePredictions = T, so that we could easily compute our across fold accuracies after the models ran. From there we used the train function eight times to run our models with our newly created training data, adjusting our data method and other respective model inputs each time. After running each of our eight models, we were able to plot the ROC curves by using the predict function with each model and set of test data, then by using the prediction function and subsequently the perfomance fucntion to realize the true positive rate and false positive rate. From there we plotted the outputs from our performance functions and distinguished model type by color, and method type by dark/light shading of each color. To compute across fold accuracies for each model, we gouped by fold and calculated the total number of correct classifications over the total number of classifcations. Next we used the confusionMatix function to collect the test accuracy for each model, as well as other diagnostics for evaluation. We ended up electing to explore the kappa coefficent for each method two model and how it related to test accuracy. The final step of our Modeling Phase was to select cutoff points for our ROC Curves. We completed this calculation with the use of a function published on R-Bloggers in the article “A small introduction to the ROCR package”, and further created a chart demonstrating the sensitivity and specificity level retained for each model with the method two data. 

Phase 4: Diagnostics

In the Diagnostics Phase we decided to do an in depth analysis of the SVM model, this time using all three classifiers (cloudy, not-cloudy, and unlabeled). We first combined our subsamples of the three images mentioned in the Data Exploration Phase, sampled 80% of the newly combined data, and set this as our training data. The remaining data was used for the test set. Then, again we used the trainControl function, setting the number of folds to five. With the use of the train function we tuned the cost and gamma parameters and then provided a plot of our model. Next we sampled 6,000 data points from our combined subsamples and made use of the tune function with range of gamma = 2^(-3:3) and range of cost 2^(0:9). We then provided a geom_tile plot of this tune and the proceeded to do PCA analysis on the first and second training sets established in the Preparation Phase of the project. We created our final models with the use of newly created training sets, taking into consideration the PCA analysis of the radiance features (DF, CF, BF, AF, AN). We landed on the features (NDAI, SD, XCoord, YCorrd, PC1, and PC2), with gamma set to 1 and cost set to four. From there we set created table for each of our final models that displayed the predicted values compared to the actual values. We decided to create visualizations for our final model using the updated method two training data. The first plot explored the XCoord and the YCoord with each of the other features sliced at the 75th quantile. The next plot looked into NDAI and SD, with the XCoord and YCoord features sliced at the 25th quantile and the other features sliced at the mean. The third plot examined CORR and DF, this time with the XCoord and YCoord features sliced at the median and the other features sliced at the mean. We finally completed PCA analysis on the test data, again with the raidance features, and created one last plot, this time with the final model that contained the updated method one training data, a sample of 6000 data points from the testing PCA analysis, an examination of the XCorrd and YCorrd, with slices at the 75th quantile for the remainging features. Finally we provided a visualization that demonstrated the run time of an SVM model with our data at 10, 100, 500, 1,000, 1,500, 2,500, 5,000, 7,500, and 10,000 observations. 
