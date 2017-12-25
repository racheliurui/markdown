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

## Decision Boundary

对于classification的数学建模，一旦模型选定了，我们在猜测一组参数后，就可以确定一条Decision boundary来区分我们的数据从而进行判断。

首先假定我们这么定义建模后的判断规则：

{% math %}
\begin{align*}& h_\theta(x) \geq 0.5 \rightarrow y = 1 \newline& h_\theta(x) < 0.5 \rightarrow y = 0 \newline\end{align*}
{% endmath %}

对原始公式进行推导，可以得出：

{% math %}
\begin{align*}& \theta^T x \geq 0 \Rightarrow y = 1 \newline& \theta^T x < 0 \Rightarrow y = 0 \newline\end{align*}
{% endmath %}

Decision Boundary的概念不光可以用于linear的建模，也适用于多次方程（polynomial function， non-liner）的建模。

## Cost Function for logistic regression model

首先尝试使用Liner regression类似的模型，发现图形不convex。于是做出改变，以符合实际情况。

{% math %}
\begin{align*}& J(\theta) = \dfrac{1}{m} \sum_{i=1}^m \mathrm{Cost}(h_\theta(x^{(i)}),y^{(i)}) \newline & \mathrm{Cost}(h_\theta(x),y) = -\log(h_\theta(x)) \; & \text{if y = 1} \newline & \mathrm{Cost}(h_\theta(x),y) = -\log(1-h_\theta(x)) \; & \text{if y = 0}\end{align*}
{% endmath %}

对它的解释和图形理解参见如下：

如果算出来的结果和实际刚好相反，那么cost为无穷大。这样就可以修正模型参数。

{% math %}
\begin{align*}& \mathrm{Cost}(h_\theta(x),y) = 0 \text{ if } h_\theta(x) = y \newline & \mathrm{Cost}(h_\theta(x),y) \rightarrow \infty \text{ if } y = 0 \; \mathrm{and} \; h_\theta(x) \rightarrow 1 \newline & \mathrm{Cost}(h_\theta(x),y) \rightarrow \infty \text{ if } y = 1 \; \mathrm{and} \; h_\theta(x) \rightarrow 0 \newline \end{align*}
{% endmath %}


一个简化版的cost 写法：

{% math %}
\mathrm{Cost}(h_\theta(x),y) = - y \; \log(h_\theta(x)) - (1 - y) \log(1 - h_\theta(x))
{% endmath %}

于是，简化版cost function：

{% math %}
J(\theta) = - \frac{1}{m} \displaystyle \sum_{i=1}^m [y^{(i)}\log (h_\theta (x^{(i)})) + (1 - y^{(i)})\log (1 - h_\theta(x^{(i)}))]
{% endmath %}

用Matrix方式表示：

{% math %}
\begin{align*} & h = g(X\theta)\newline & J(\theta) = \frac{1}{m} \cdot \left(-y^{T}\log(h)-(1-y)^{T}\log(1-h)\right) \end{align*}
{% endmath %}

## 根据Cost Function来确定logistic regression模型的Gradient Descent 算法

{% math %}
\begin{align*} & Repeat \; \lbrace \newline & \; \theta_j := \theta_j - \frac{\alpha}{m} \sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)}) x_j^{(i)} \newline & \rbrace \end{align*}
{% endmath %}

使用Matrix表示（vectorized implementation）：

{% math %}
\theta := \theta - \frac{\alpha}{m} X^{T} (g(X \theta ) - \vec{y})
{% endmath %}

## 比Gradient Decent更高级的算法

"Conjugate gradient", "BFGS", and "L-BFGS" 。
优点： 不需要提供learning rate（自适应）；快
缺点： 复杂，一般人只能做api caller

调用方法：

先实现模型的cost算法 和 Gradient Decent中用的derivation的算法。

```octave
function [jVal, gradient] = costFunction(theta)
  jVal = [...code to compute J(theta)...];
  gradient = [...code to compute derivative of J(theta)...];
end
```

作为参数告诉高级算法。

```Octave
options = optimset('GradObj', 'on', 'MaxIter', 100);
initialTheta = zeros(2,1);
   [optTheta, functionVal, exitFlag] = fminunc(@costFunction, initialTheta, options);
```

## MultiClass classification

one-vs-rest
用同样的算法，每次求参数的时候，把当前class的剩余类别看成一个虚拟同类。然后求出当前class的参数。

数学表示：

{% math %}
\begin{align*}& y \in \lbrace0, 1 ... n\rbrace \newline& h_\theta^{(0)}(x) = P(y = 0 | x ; \theta) \newline& h_\theta^{(1)}(x) = P(y = 1 | x ; \theta) \newline& \cdots \newline& h_\theta^{(n)}(x) = P(y = n | x ; \theta) \newline& \mathrm{prediction} = \max_i( h_\theta ^{(i)}(x) )\newline\end{align*}
{% endmath %}


# Solving "Over fitting" problem
