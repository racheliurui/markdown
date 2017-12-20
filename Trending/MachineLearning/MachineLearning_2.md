title: Machine Learning - Week 2
mathjax: true
date: 2017-12-20 06:31:00
tags:
- Machine Learning
---

https://www.coursera.org/learn/machine-learning/home/week/2

## Multivariate Linear Regression

上周学的是单个参数的Linear Regression， 模型中只有一个变量x。Multivariate Linear Regression是，

{% math %}
h_\theta (x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_3 + \cdots + \theta_n x_n
{% endmath %}

具体描述：

{% math %}
\begin{align*}x_j^{(i)} &= \text{value of feature } j \text{ in the }i^{th}\text{ training example} \newline x^{(i)}& = \text{the input (features) of the }i^{th}\text{ training example} \newline m &= \text{the number of training examples} \newline n &= \text{the number of features} \end{align*}
{% endmath %}

适用Matrix表示就变成，

{% math %}
\begin{align*}h_\theta(x) =\begin{bmatrix}\theta_0 \hspace{2em} \theta_1 \hspace{2em} ... \hspace{2em} \theta_n\end{bmatrix}\begin{bmatrix}x_0 \newline x_1 \newline \vdots \newline x_n\end{bmatrix}= \theta^T x\end{align*}
{% endmath %}

其中，

{% math %}
x_{0}^{(i)} =1 \text{ for } (i\in { 1,\dots, m } )
{% endmath %}

## Gradient Descent for Multiple Variables

对于多参数的Linear Regression模型，求最优参数的算法相应就叫做，Gradient Descent for Multiple Variables。 运用前面的知识，其表示就写为：

{% math %}
\begin{align*}& \text{repeat until convergence:} \; \lbrace \newline \; & \theta_j := \theta_j - \alpha \frac{1}{m} \sum\limits_{i=1}^{m} (h_\theta(x^{(i)}) - y^{(i)}) \cdot x_j^{(i)} \; & \text{for j := 0...n}\newline \rbrace\end{align*}
{% endmath %}

### Gradient Descent in Practice I - Feature Scaling

一种方式是把模型中的训练数据的范围调整成放大或者缩小为同样大小。这样算法的复杂度好控制。
−1 ≤ x(i) ≤ 1
or
−0.5 ≤ x(i) ≤ 0.5

方法叫做feature scaling 或者 mean normalization：

{% math %}
x_i := \dfrac{x_i - \mu_i}{s_i}
{% endmath %}

{% math %}\μ_i{% endmath %} is the average of all the values for feature (i) and \s_i is the range of values (max - min), or s_i is the standard deviation.

### Gradient Descent in Practice II - Learning Rate

如何选择最合适的learning rate参数？

Debugging gradient descent： 跑若干遍，如果J(θ)反而变大，那么说明这个参数太大了（步子太大，miss了最优点）

Automatic convergence test：实际情况下，选定两次运行J(θ)比较结果，如果差距小于比如0.001，则说明是在收敛.但是具体情况具体分析。

 α is sufficiently small, then J(θ) will decrease on every iteration.

## Features and Polynomial Regression

Feature在ML里面指的是我们拿到的数据参数。比如房子的宽度算是一个feature。
有时候，为了fitin 模型，我们可以把参数二合一，比如不用长宽，而是用相乘得到的面积作为一个模型的feature。

Polynomial Regression
这是非liner regression。例如：

* quadratic function

{% math %}
h_\theta(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_1^2
{% endmath %}

* cubic function

{% math %}
h_\theta(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_1^2 + \theta_3 x_1^3
{% endmath %}

* square root function

{% math %}
h_\theta(x) = \theta_0 + \theta_1 x_1 + \theta_2 \sqrt{x_1}
{% endmath %}

需要建立的概念是以上每种模型的大致图形应当符合我们收集的数据。有算法帮助我们选择模型。

使用这些模型的时候feature scaling非常重要，因为平方或者多次方运算后的结果会很大或者很小。

# Normal Equation
