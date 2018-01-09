title: Machine Learning - Week 6
mathjax: true
date: 2018-01-09 21:43:11
tags:
- Machine Learning
- Neron Network
---

# What do to next

如果当前的建模不够好，误差很大，怎么办？有以下Options

* 收集更多训练数据
* 减少无关feature
* 增加相关feature
* 组合，变形，使用Polynomial feature
* 减小lambda
* 增大lambda

## 如何评估模型？

### 数据七三开

把数据七三开（随机筛选分组）。使用七成数据训练参数，三成数据评估。

对于Liner Regression来说，评估使用的公式是：
{% math %}
J_{test}(\Theta) = \dfrac{1}{2m_{test}} \sum_{i=1}^{m_{test}}(h_\Theta(x^{(i)}_{test}) - y^{(i)}_{test})^2
{% endmath %}

对于Logical Regression来说，单组数据的错误是这样评估的：
{% math %}
err(h_\Theta(x),y) = \begin{matrix} 1 & \mbox{if } h_\Theta(x) \geq 0.5\ and\ y = 0\ or\ h_\Theta(x) < 0.5\ and\ y = 1\newline 0 & \mbox otherwise \end{matrix}
{% endmath %}

那么对于多组评估数据来说，其评估公式为：
{% math %}
\text{Test Error} = \dfrac{1}{m_{test}} \sum^{m_{test}}_{i=1} err(h_\Theta(x^{(i)}_{test}), y^{(i)}_{test})
{% endmath %}

其中心思想是：
对于随机分组的数据来说，正确的算法的cost function表现应该一致且小。 如果表现不一致说明可能over fitting了，如果不够小，说明模型不契合。

### 更进一步六二二开


中心思想，数据随机分组。
Training set用来训练参数。（训练多个不同的模型，每个模型都得到一组参数）
Cross Validation set用来选择Polynomial模型（选最小的）
Test Set用来验证模型。（用选择的Polynomial degree的模型测试cost function还是很小）。
