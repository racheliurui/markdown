title: Machine Learning - Week 1
mathjax: true
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

cost function for liner regression model
=Squared error function
=Mean squared error


$$
J(\theta_0, \theta_1) = \dfrac {1}{2m} \displaystyle \sum _{i=1}^m \left ( \hat{y}_{i}- y_{i} \right)^2 = \dfrac {1}{2m} \displaystyle \sum _{i=1}^m \left (h_\theta (x_{i}) - y_{i} \right)^2
$$

对于Liner Regression来说（h(x)=θ_0+θ_1x），它的模型图形总是一个bowl shaped，又叫convex function.

### Cost Function - Intuition I

假设我们的h模型是h(x)=θ_1x，
假设模型看起来是对的，但是我们还不知道正确的θ值，那么尝试θ值的过程中，每次尝试我们都可以记录cost function 的值的变化，假设我们模型（h(x)=θ_1x）是正确的情况下，记录θ_1值的变化和cost function的关系，就是一个倒抛物线，最低点就是我们要找的θ值。

所以找θ_1就变成，我们假定我们的模型是正确的，那么我们的数据点应该分布在模型上，给定一个猜测的θ_1，我们使用cost function来衡量误差（坡度），然后根据Gradient Descent算法向着正确的最优点移动（learning rate + slope)。



### Cost Function - Intuition II
假设我们的h模型是h(x)=θ_0+θ_1x
假设模型看起来是对的，但是我们还不知道正确的θ值，那么尝试θ0和θ1值的过程中，每次尝试我们都可以记录cost function 的值的变化，它是一个3D的倒抛物线网，体现了最低点就是我们要找的θ0和θ1值。

从另一个角度看，如果横轴是θ0， 纵轴是θ1，然后用线表示θ0和θ1值组合导致一样结果的cost function J(θ)的话，这个图会很像星系旋转的图。而星系旋转图的中心点，就是我们追寻的正确的θ0和θ1值。因为在那里cost function的值最小。

## Gradient Descent

Cost function是用来衡量我们的模型和参数的效果。Gradient Descent是一种算法帮我们找到最好的参数。

在这个课程中，
:= assignment
= truth assertion

gradient descent算法的表达：

repeat until convergence:
$$
\theta_1:=\theta_1-\alpha \frac{d}{d\theta_1} J(\theta_1)
$$


Gradient Descent For Linear Regression：

$$
\begin{align*} \text{repeat until convergence: } \lbrace & \newline \theta_0 := & \theta_0 - \alpha \frac{1}{m} \sum\limits_{i=1}^{m}(h_\theta(x_{i}) - y_{i}) \newline \theta_1 := & \theta_1 - \alpha \frac{1}{m} \sum\limits_{i=1}^{m}\left((h_\theta(x_{i}) - y_{i}) x_{i}\right) \newline \rbrace& \end{align*}
$$

上面的公式中，\alpha 表示的是learning rate ， 太大的话，可能永远找到最佳点，如果太小，可能花很长时间。

以下部分表示的是曲线的坡度（slope）。

$$
\frac{\partial}{\partial \theta_j}
$$

坡度越小，越接近local minimum（最佳点），Gradient Descent会自动降低step大小，因为上面公式算出来的值越小。另外，该值还体现了坡度的正负，以修正正确的移动方向。
参见以下网址复习这种算法的原理：
https://www.coursera.org/learn/machine-learning/supplement/QKEdR/gradient-descent-intuition



当前介绍的gradient descent算法属于Batch gradient descent。它是gradient descent的细分算法，它算得时候采用完整已知数据集来计算。有些其它gradient descent算的时候是采用部分数据集的。



## 概念小结

* Linier Regression是一个模型，是我们对数据规律的一种猜测。它属于适用于Supervised Learning中的Regression中的一种模型。
* Cost Function是一个衡量模型中的参数合理性的标准。本节中学习的Squared error function是针对liner regression model的cost function。
* Gradient Descent是一种求最佳参数的算法。它特别针对的也是Squared error function做为衡量标准时候帮助求最优参数的算法。

## Matrice and Vectors

### 基本概念

表达Matrix大小是row×colume， 比如3*2的matrix就是3行2列。数学表示是R加上row×colume的右上标
表达Matrix中的数据，用下角标row,colume

Vector是只有1列的matrix (a n×1 matrix)， 数学表达是R加上Row的右上标。

Scalar： row和column都等于1

### Matrix运算

加减法： 同位置的加减；参与运算的Matrix必须一样大小
乘除scalar：每个元素均和scalar相乘除
Matrix-Vector乘法：An m x n matrix multiplied by an n x 1 vector results in an m x 1 vector.
Matrix-Matrix乘法：An m x n matrix multiplied by an n x o matrix results in an m x o matrix.
第一个matix的行乘以第二个matrix的列

### Matrix运算的特点
A×B 不等于B×A
A×B×C 等于A×(B×C)
identical matrix是一个左上到右下对角线是1，剩余位置是0的特殊Matix。因为Matrix和identical matrix相乘后结果不变。

### 特殊的Matrix
Inverse Matrix：一定是一个m×m的正方形的Matrix
Matrix×(InverseMatrix)=identicalMatrix
如果Matrix没有inverse，比如全零，则叫做singular或者degenerateMatrix

Transpose Matrix： 行变列
