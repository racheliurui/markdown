title: Machine Learning
date: 2017-09-24 17:50:23
tags:
- Machine Learning
---

https://www.coursera.org/learn/c/lecture/RKFpn/welcome


# Overview

## Example of machine Learning

 * Database mining
 * Application can't program by Hand
 * Self-customizing programs
 * Understanding Human Learning (Brain, Real AI)

## What is Machine Learning

Two definitions of Machine Learning are offered.

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

Cost function is to measure the accuracy of our hypothesis function.

J(\theta_0, \theta_1) = \dfrac {1}{2m} \displaystyle \sum _{i=1}^m \left ( \hat{y}_{i}- y_{i} \right)^2 = \dfrac {1}{2m} \displaystyle \sum _{i=1}^m \left (h_\theta (x_{i}) - y_{i} \right)^2

cost function
=Squared error function
=Mean squared error

### Cost Function - Intuition I

假设我们的h模型是h(x)=θ_1x，
假设模型看起来是对的，但是我们还不知道正确的θ值，那么尝试θ值的过程中，每次尝试我们都可以记录cost function 的值的变化，它是一个倒抛物线，体现了最低点就是我们要找的θ值。



### Cost Function - Intuition II
假设我们的h模型是h(x)=θ_0+θ_1x
假设模型看起来是对的，但是我们还不知道正确的θ值，那么尝试θ0和θ1值的过程中，每次尝试我们都可以记录cost function 的值的变化，它是一个3D的倒抛物线网，体现了最低点就是我们要找的θ0和θ1值。

从另一个角度看，如果横轴是θ0， 纵轴是θ1，然后用线表示θ0和θ1值组合导致一样结果的cost function J(θ)的话，这个图会很像星系旋转的图。而星系旋转图的中心点，就是我们追寻的正确的θ0和θ1值。因为在那里cost function的值最小。

## Gradient Descent

Cost function是用来衡量我们的模型和参数的效果。Gradient Descent是一种__算法__帮我们找到最好的参数。

在这个课程中，
:= assignment
= truth assertion

\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta_0, \theta_1)
