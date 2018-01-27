title: Machine Learning - Week 9
mathjax: true
date: 2018-01-27 12:54:16
tags:
- Machine Learning
- Density Estimation
- Gaussian Distribution
---

# Anomaly Detection

Anomaly Detection 不规则检测。举例子：飞机引擎的各种参数，如果一个新的引擎的发热或者其它参数突然与众不同，那么我们肯定担心有什么问题。

应用场景：
Fraud Detection
Manufactoring

根据data建模p(x)，当新的x造成{% math %}p(x)<$$\epsilon$${% endmath%}的时候，就是说明数据异常。

## Gaussian (normal) Distribution

公式表达：

{%math%}
\begin{align*}
X\thicksim N(\mu,\sigma^2)
\end{align*}
{%endmath%}

{%math%}\thicksim{%endmath%} 读作“distributed as”，N是normal的意思，$$\mu$$代表正态分布的最高点投射到横轴的读数，$$\sigma$$表示的是正态分布的驼峰的宽度。$$/sigma$$又叫standard deviation.

{%math%}
\begin{align*}
p(x;\mu,\sigma^2)=\frac{1}{\sqrt{2\pi }\sigma }exp(-\frac{(x-\mu)^2}{\sigma^2})
\end{align*}
{%endmath%}


For a dataset,
{%math%}
\begin{align*}
\{x^\left(1\right),x^\left(2\right),...x^\left(m\right)\}, x\left(i\right)\in\Re
\end{align*}
{%endmath%}

有以下求值公式来推导已知数据的正态分布：

{%math%}
\begin{align*}
\mu=\frac{1}{m}\displaystyle\sum\limits_{i=1}^m x^\left(i\right)
\end{align*}
{%endmath%}

{%math%}
\begin{align*}
\sigma ^2=\frac{1}{m}\displaystyle\sum\limits_{i=1}^m (x^\left(i\right)-\mu )^2
\end{align*}
{%endmath%}

* 在统计分析课中，有时候除以m会写成除以（m-1），在machine learning中，m通常很大，所以我们忽略这种不同。
