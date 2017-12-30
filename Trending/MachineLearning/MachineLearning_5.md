title: Machine Learning - Week 5
mathjax: true
date: 2017-12-26 16:05:21
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

* k>=3的话，根据最初的模型，也就说明一组待预测数据进去，出来的最终结果是一个vector，这表明最终结果反映的是k属于每个class分类可能性的大小。（k=1反映的是一组数据进去，最终结果是0或者1，一个unit即可表达）

* Cost function for regularized logistic regression

{% math %}
J(\theta) = - \frac{1}{m} \sum_{i=1}^m [ y^{(i)}\ \log (h_\theta (x^{(i)})) + (1 - y^{(i)})\ \log (1 - h_\theta(x^{(i)}))] + \frac{\lambda}{2m}\sum_{j=1}^n \theta_j^2
{% endmath %}

* Cost function for neural networks
{% math %}
\begin{gather*} J(\Theta) = - \frac{1}{m} \sum_{i=1}^m \sum_{k=1}^K \left[y^{(i)}_k \log ((h_\Theta (x^{(i)}))_k) + (1 - y^{(i)}_k)\log (1 - (h_\Theta(x^{(i)}))_k)\right] + \frac{\lambda}{2m}\sum_{l=1}^{L-1} \sum_{i=1}^{s_l} \sum_{j=1}^{s_{l+1}} ( \Theta_{j,i}^{(l)})^2\end{gather*}
{% endmath %}
