title: Machine Learning - Week 5
mathjax: true
date: 2017-12-30 21:05:21
tags:
- Machine Learning
- Neron Network
---


# Neron Network的cost function

Neron Network的cost function是logic regression的加强版。

先熟悉一些term

L = network的总层数
{% math %}s_l{% endmath %} = 层l的unit个数（不包含bias unit）
K = 最后一层的unit个数（其实反映的是classes的个数）

* k>=3的话，根据最初的模型，也就说明一组待预测数据进去，出来的最终结果是一个vector，这表明最终结果反映的是待预测的输入属于每个class分类可能性的大小（比如图片分类）。（k=1反映的是一组数据进去，最终结果是0或者1，一个unit即可表达）

* Cost function for regularized logistic regression

{% math %}
J(\theta) = - \frac{1}{m} \sum_{i=1}^m [ y^{(i)}\ \log (h_\theta (x^{(i)})) + (1 - y^{(i)})\ \log (1 - h_\theta(x^{(i)}))] + \frac{\lambda}{2m}\sum_{j=1}^n \theta_j^2
{% endmath %}

* Cost function for neural networks
{% math %}
\begin{gather*} J(\Theta) = - \frac{1}{m} \sum_{i=1}^m \sum_{k=1}^K \left[y^{(i)}_k \log ((h_\Theta (x^{(i)}))_k) + (1 - y^{(i)}_k)\log (1 - (h_\Theta(x^{(i)}))_k)\right] + \frac{\lambda}{2m}\sum_{l=1}^{L-1} \sum_{i=1}^{s_l} \sum_{j=1}^{s_{l+1}} ( \Theta_{j,i}^{(l)})^2\end{gather*}
{% endmath %}

# Neron Network的求值算法 Back Propagation Algorithm

给定一组训练数据如下：

{% math %}
\lbrace (x^{(1)}, y^{(1)}) \cdots (x^{(m)}, y^{(m)})\rbrace
{% endmath %}

初始化：

{% math %}
\Delta^{(l)}_{i,j}
{% endmath %}


Loop开始（pick一组训练数据）
* * *

* 第一步，根据模型，使用 forward propagation 求出当前训练数据的预测值
* 从{% math %}\delta^{(L)} = a^{(L)} - y^{(t)}{% endmath %}开始，使用Back propagation 算出所有的{% math %}\delta{% endmath %}

其中，算法公式为：
{% math %}\delta^{(l)} = ((\Theta^{(l)})^T \delta^{(l+1)})\ .*\ a^{(l)}\ .*\ (1 - a^{(l)}){% endmath %}

* 第二步，计算每一层的delta

{% math %}
g'(z^{(l)}) = a^{(l)}\ .*\ (1 - a^{(l)})
{% endmath %}

注意在一些表达中出现了如上公式，叫做g-prime derivative terms。


{% math %}
\Delta^{(l)}_{i,j} := \Delta^{(l)}_{i,j} + a_j^{(l)} \delta_i^{(l+1)}
{% endmath %}

写为
{% math %}
\Delta^{(l)} := \Delta^{(l)} + \delta^{(l+1)}(a^{(l)})^T
{% endmath %}

* 第三步，累计每一层的大delta，从而得出overall的partial derivative公式。

j不等于0时：
{% math %}
D^{(l)}_{i,j} := \dfrac{1}{m}\left(\Delta^{(l)}_{i,j} + \lambda\Theta^{(l)}_{i,j}\right)
{% endmath %}
j等于0时：
{% math %}
D^{(l)}_{i,j} := \dfrac{1}{m}\Delta^{(l)}_{i,j}
{% endmath %}

最后得出Partial Derivative是：
{% math %}
\frac \partial {\partial \Theta_{ij}^{(l)}} J(\Theta)
{% endmath %}

* * *
Loop结束


back propagation实际在做什么？

实际上是从结果的delta倒推每一层每个参数的delta。
每个单元的delta其实是用当前单元向前propagate时候贡献过的单元的delta和贡献时候使用的theta值结合起来算出来的。
