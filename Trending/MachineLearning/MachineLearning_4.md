title: Machine Learning - Week 4
mathjax: true
date: 2017-12-26 16:05:21
tags:
- Machine Learning
- Neron Network
---

# Non-liner overview

图像处理的例子。像素50*50的时候，每个像素点作为一个feature，

* 黑白图片的话，会一共有50×50个feature
* 彩色图片的话，红黄蓝三色分开算，共有50*50*3个feature

如果采用quadratic features的方式建模(即每个参数均由任意两个feature相乘得来)，xi* xj，

n个数，两两排列组合，可能性是n*n/2，所以feature变成约 3 million

** 结论：Non-liner的问题，建模遇到的问题往往是feature太多。所以我们引入神经网络，会帮助大大简化问题。 **

## Neron and brain

科学家发现，大脑的区域经过信号的训练可以对相应的信息进行处理。比如把视觉神经搭到听觉区域，那么听觉区域通过长期的信号训练可以发展出对视觉信息的处理能力。

因此，类似实验有：
* 把图像信号从舌头输入训练触觉区的神经元来处理图像。
* 把声音信号输入用来训练神经元进行处理回声定位处理。（例子，没有眼球的小孩儿通过回声判断周围情况）
* 用腰带信号来定位北方，训练人类神经元具备和鸟类一样的方向感。
* 给青蛙植入一个额外的眼睛，并训练神经元学会使用额外的眼睛。

### Neron network

At a very simple level, neurons are basically computational units that take inputs (dendrites) as electrical inputs (called "spikes") that are channeled to outputs (axons). In our model, our dendrites are like the input features x1⋯xn, and the output is the result of our hypothesis function. In this model our x0 input node is sometimes called the "bias unit." It is always equal to 1. In neural networks, we use the same logistic function as in classification, 11+e−θTx, yet we sometimes call it a sigmoid (logistic) activation function. In this situation, our "theta" parameters are sometimes called "weights".

基本的神经元表示： 若干输入，根据算法，得到输出。

{% math %}
\begin{bmatrix}x_0 \newline x_1 \newline x_2 \newline \end{bmatrix}\rightarrow\begin{bmatrix}\ \ \ \newline \end{bmatrix}\rightarrow h_\theta(x)
{% endmath %}
