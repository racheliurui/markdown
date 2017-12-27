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

At a very simple level, neurons are basically computational units that take __inputs (dendrites)__ as electrical inputs __(called "spikes")__ that are channeled to outputs (axons). In our model, our dendrites are like the input features x1⋯xn, and the output is the result of our hypothesis function. In this model our __x0 input node is sometimes called the "bias unit."__ It is always equal to 1. In neural networks, we use the same logistic function as in classification, 11+e−θTx, yet we sometimes call it a __sigmoid (logistic) activation function__. In this situation, our "theta" parameters are sometimes called "weights".

基本的神经元表示： 若干输入，根据算法，得到输出。

{% math %}
\begin{bmatrix}x_0 \newline x_1 \newline x_2 \newline \end{bmatrix}\rightarrow\begin{bmatrix}\ \ \ \newline \end{bmatrix}\rightarrow h_\theta(x)
{% endmath %}

每一层的神经元和参数表示为：

{% math %}
\begin{align*}& a_i^{(j)} = \text{"activation" of unit $i$ in layer $j$} \newline& \Theta^{(j)} = \text{matrix of weights controlling function mapping from layer $j$ to layer $j+1$}\end{align*}
{% endmath %}

例如，一个神经网络表示为：（最左边的一层是input layer，最右边是output layer，中间是hidden layer）。

{% math %}
\begin{bmatrix}x_0 \newline x_1 \newline x_2 \newline x_3\end{bmatrix}\rightarrow\begin{bmatrix}a_1^{(2)} \newline a_2^{(2)} \newline a_3^{(2)} \newline \end{bmatrix}\rightarrow h_\theta(x)
{% endmath %}

每一层的神经元求值公式是：

{% math %}
\begin{align*} a_1^{(2)} = g(\Theta_{10}^{(1)}x_0 + \Theta_{11}^{(1)}x_1 + \Theta_{12}^{(1)}x_2 + \Theta_{13}^{(1)}x_3) \newline a_2^{(2)} = g(\Theta_{20}^{(1)}x_0 + \Theta_{21}^{(1)}x_1 + \Theta_{22}^{(1)}x_2 + \Theta_{23}^{(1)}x_3) \newline a_3^{(2)} = g(\Theta_{30}^{(1)}x_0 + \Theta_{31}^{(1)}x_1 + \Theta_{32}^{(1)}x_2 + \Theta_{33}^{(1)}x_3) \newline h_\Theta(x) = a_1^{(3)} = g(\Theta_{10}^{(2)}a_0^{(2)} + \Theta_{11}^{(2)}a_1^{(2)} + \Theta_{12}^{(2)}a_2^{(2)} + \Theta_{13}^{(2)}a_3^{(2)}) \newline \end{align*}
{% endmath %}

{% math %}
\text{If network has $s_j$ units in layer $j$ and $s_{j+1}$ units in layer $j+1$, then $\Theta^{(j)}$ will be of dimension $s_{j+1} \times (s_j + 1)$.}
{% endmath %}

想象一下多层的网状结构，每一层都有一个隐含变量，所以当前层用前一层求值的时候，参数永远都是一个Matrix，行数等于当前层的单元数，列数等于（前一层变量数+1)。

### Neron network的简化表达

算法：
{% math %}
z^{(j)} = \Theta^{(j-1)}a^{(j-1)}
{% endmath %}


{% math %}
a^{(j)} = g(z^{(j)})
{% endmath %}


### Neron network in simple action


以下是使用Neron Network建模的AND function（参数已经预先fit in，需要记住g(4)=0.99 ； g(-4.6)=0.01):

{% math %}
\begin{align*}& h_\Theta(x) = g(-30 + 20x_1 + 20x_2) \newline \newline & x_1 = 0 \ \ and \ \ x_2 = 0 \ \ then \ \ g(-30) \approx 0 \newline & x_1 = 0 \ \ and \ \ x_2 = 1 \ \ then \ \ g(-10) \approx 0 \newline & x_1 = 1 \ \ and \ \ x_2 = 0 \ \ then \ \ g(-10) \approx 0 \newline & x_1 = 1 \ \ and \ \ x_2 = 1 \ \ then \ \ g(10) \approx 1\end{align*}
{% endmath %}


### Neron network in simple action II

XNOR使用2层Neron network构建。
（结合之前一层network的结果得出network的结构）。
{% math %}
\Theta^{(1)} =\begin{bmatrix}-30 & 20 & 20 \newline 10 & -20 & -20\end{bmatrix}
{% endmath %}

{% math %}
\begin{align*}& a^{(2)} = g(\Theta^{(1)} \cdot x) \newline& a^{(3)} = g(\Theta^{(2)} \cdot a^{(2)}) \newline& h_\Theta(x) = a^{(3)}\end{align*}
{% endmath %}

## 结果是multiclass的Neron Network

前面的例子中，最终的output是0或者1，是单个结果。如果结果是multiclass呢？
比如：识别手写数字的例子中，结果是0-9 一共10种可能，这种情况下，最终的output不是一维的，而是多维的。

举个栗子，这是一个四个class（multiclass)的单个结果的例子：
{% math %}
h_\Theta(x) =\begin{bmatrix}0 \newline 0 \newline 1 \newline 0 \newline\end{bmatrix}
{% endmath %}

所以，对于这种模型，上一层theta参数的个数也要再乘以单个结果vector维度。
