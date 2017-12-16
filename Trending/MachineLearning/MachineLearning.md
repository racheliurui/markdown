title: Machine Learning
date: 2017-09-24 17:50:23
tags:
- Machine Learning
---

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>


https://www.coursera.org/learn/c/lecture/RKFpn/welcome


# Overview

## Example of machine Learning

 * Database mining
 * Application can't program by Hand
 * Self-customizing programs
 * Understanding Human Learning (Brain, Real AI)

## What is Machine Learning

Two definitions of Machine Learning are offered.

* Arthur Samuel described it as: "the field of study that gives computers the ability to learn without being explicitly programmed." This is an older, informal definition.

* Tom Mitchell provides a more modern definition: "A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E."

Example: playing checkers.

E = the experience of playing many games of checkers

T = the task of playing checkers.

P = the probability that the program will win the next game.

* In general, any machine learning problem can be assigned to one of two broad classifications:

Supervised learning and Unsupervised learning.

* others include reinforcement learning and recommender systems


## Supervised Learning

__告诉机器，这是数据，这是结果，你学习一下，以后再有数据请告诉我结果。__
分为两种，
   * regression： 这是数据， 它和结果的关系我猜测是线性的。
     比如， 根据预定的参数模型预测房价
   * classification：这是数据， 它和结果的关系是非线性的（离散的），请帮我预测界限。
     比如， 根据预定的参数模型，预测肿瘤是恶性还是良性（离散结果）

## Unsupervised Learning

__告诉机器，这是数据，你学习一下，找出他们之间有什么关系，什么分类。__

分为两种，
   * Clustering： 数据分组。 例如基因分类
   * Non-clustering： "Cocktail Party Algorithm"，用来分析音源不同的声音数据并把数据给分开。

## Model Representation

h 代表hypothesis
h : X → Y so that h(x) is a “good” predictor for the corresponding value of y.

模型举例：
h(x)=θ_0+θ_1x
__Liner Regression with one variable = Univariate Liner Regression__



## Cost Function

cost function is to measure the accuracy of our hypothesis function.

J(\theta_0, \theta_1) = \dfrac {1}{2m} \displaystyle \sum _{i=1}^m \left ( \hat{y}_{i}- y_{i} \right)^2 = \dfrac {1}{2m} \displaystyle \sum _{i=1}^m \left (h_\theta (x_{i}) - y_{i} \right)^2


minimize{θ_0,θ_1}

Square error cost functions.
We can  This takes an average difference (actually a fancier version of an average) of all the results of the hypothesis with inputs from x's and the actual output y's.

J(θ0,θ1)=12m∑i=1m(y^i−yi)2=12m∑i=1m(hθ(xi)−yi)2
To break it apart, it is 12 x¯ where x¯ is the mean of the squares of hθ(xi)−yi , or the difference between the predicted value and the actual value.

This function is otherwise called the "Squared error function", or "Mean squared error". The mean is halved (12) as a convenience for the computation of the gradient descent, as the derivative term of the square function will cancel out the 12 term. The following image summarizes what the cost function does:


When the target variable that we’re trying to predict is continuous, such as in our housing example, we call the learning problem a regression problem. When y can take on only a small number of discrete values (such as if, given the living area, we wanted to predict if a dwelling is a house or an apartment, say), we call it a classification problem.

## Cost Function - Intuition I

If we try to think of it in visual terms, our training data set is scattered on the x-y plane. We are trying to make a straight line (defined by hθ(x)) which passes through these scattered data points.

Our objective is to get the best possible line. The best possible line will be such so that the average squared vertical distances of the scattered points from the line will be the least. Ideally, the line should pass through all the points of our training data set. In such a case, the value of J(θ0,θ1) will be 0. The following example shows the ideal situation where we have a cost function of 0.


When θ1=1, we get a slope of 1 which goes through every single data point in our model. Conversely, when θ1=0.5, we see the vertical distance from our fit to the data points increase.


This increases our cost function to 0.58. Plotting several other points yields to the following graph:


Thus as a goal, we should try to minimize the cost function. In this case, θ1=1 is our global minimum.
