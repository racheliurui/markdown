title: Machine Learning - Week 3
mathjax: true
date: 2017-12-24 11:51:11
tags:
- Machine Learning
- classification
---


为什么我们不能用liner regression的建模方法在classification的问题中？
1） 结果和训练集都是离散的
2） 结果是0或者1，liner regression的模型很难fit in这样的曲线。

# 对classification的问题进行数学建模

引入概念g(z)：
Sigmoid function = logistic function
这个方程的图形决定了其区间在0和1之间。

{% math %}
\begin{align*}& h_\theta (x) = g ( \theta^T x ) \newline \newline& z = \theta^T x \newline& g(z) = \dfrac{1}{1 + e^{-z}}\end{align*}
{% endmath %}


模型引申的公理：

{% math %}
\begin{align*}& h_\theta(x) = P(y=1 | x ; \theta) = 1 - P(y=0 | x ; \theta) \newline& P(y = 0 | x;\theta) + P(y = 1 | x ; \theta) = 1\end{align*}
{% endmath %}
