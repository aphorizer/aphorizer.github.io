---
layout: post
title: "What species does this iris flower belong to?"
excerpt: "Using the publicly available iris dataset, I will use its features to predict the species the flower belongs to"
date: "July 5, 2015"
tags: 
- R
- iris
- CART
- randomForest
categories: [R]
comments: true
status: publish
output: html_document
---
 
#### In progress...
 
There are 4 variables describing the various features of the flower, one variable that identifies the flower's species and a total of 150 flowers in our dataset.
 
 
We first look at the structure of the dataset:

{% highlight r %}
str(iris)
{% endhighlight %}



{% highlight text %}
## 'data.frame':	150 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
{% endhighlight %}
 
Alright, so we are going to use the 4 predictors (Sepal.Length, Sepal.Width, Petal.Length, Petal.Width) in a CART tree followed by a randomForest. We will also explore the use of k-fold cross validation and try to include 'derived' features and remove some unneeded features and see the impact on our outcome.
 
#### Splitting data into a training set and a test set
 
First, let's split the dataset into a training set and a test set using the caTools library
 
Use ```install.packages('caTools')``` if you don't already have the library
 

{% highlight r %}
library(caTools) # load the library
 
# We are going to put 60% of the flower observations into the training set
# sample.split() ensures that our test set and training set have an equal number of flowers
# from different species. A logical vector is produced of same length as iris$Species
set.seed(111)
SplitVector <- sample.split(iris$Species, SplitRatio = 0.6)
 
# Now we break our iris dataset into the following two datasets
# The observations for which SplitVector is TRUE go to the training set and the rest
# go to the test set
irisTrain <- iris[SplitVector,] 
irisTest <- iris[!SplitVector,]
{% endhighlight %}

{% highlight r %}
table(irisTrain$Species)
{% endhighlight %}



{% highlight text %}
## 
##     setosa versicolor  virginica 
##         30         30         30
{% endhighlight %}



{% highlight r %}
table(irisTest$Species)
{% endhighlight %}



{% highlight text %}
## 
##     setosa versicolor  virginica 
##         20         20         20
{% endhighlight %}
 
As you can see, the test set has 20 flowers from each species and the training set has 30 flowers from each species. This ensures that there is no selection bias in our trianing set.
It is a representative sample with equal percentages (33%) of flowers in each species category.
 
#### CART tree (Classification and Regression Tree)
 
We are going to use the ```rpart``` and ```rpart.plot``` packages for this problem
 

{% highlight r %}
library(rpart)
library(rpart.plot)
# I set the method to 'class' telling rpart() that this will
# will be a classification tree not a regression tree.
CART.model <- rpart(Species ~., data = irisTrain, method = 'class')
prp(CART.model) # view the tree
{% endhighlight %}

![plot of chunk unnamed-chunk-4](/figures/unnamed-chunk-4-1.png) 
 
It turns out that our model only uses two features, namely `Petal.Length` and `Petal.Width`.
 
##### Interpretation of the tree:
 
If the petal length of the flower is < 2.5cm then the plant will be classified as belonging to the 'setosa' species, otherwise it will go on to check the petal width. If the petal width is < 1.6 cm then the plant will be classified as a 'versicolor'. Otherwise it will classify the flower as an Iris 'virginica'.
 
Let's perform our predictions:
 

{% highlight r %}
# specifying type to 'class' tells predict() to return the results as 
# classifications not probabilities
predict.CART <- predict(CART.model, newdata = irisTest, type = 'class')
table(irisTest$Species, predict.CART )
{% endhighlight %}



{% highlight text %}
##             predict.CART
##              setosa versicolor virginica
##   setosa         20          0         0
##   versicolor      0         18         2
##   virginica       0          2        18
{% endhighlight %}
 
Nice, our model predicted with a high accuracy and in total made only 4 errors as is shown in the confusion matrix printed out.
 
Formally,
 

{% highlight r %}
(accuracy = (20+18+18)/nrow(irisTest))
{% endhighlight %}



{% highlight text %}
## [1] 0.9333333
{% endhighlight %}
 
 
#### Cross-validating our CART tree
 
 
#### Random Forest
 
#### Test set Accuracies
 
#### Feature Analysis
 
#### Rebuilding CART tree and Random Forest
 
#### Accuracy Improvements?
