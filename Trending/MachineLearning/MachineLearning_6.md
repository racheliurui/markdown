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


### 一些概念



High bias (underfitting): both Jtrain(Θ) and JCV(Θ) will be high. Also, JCV(Θ)≈Jtrain(Θ).

High variance (overfitting): Jtrain(Θ) will be low and JCV(Θ) will be much greater than Jtrain(Θ).

## 如何结合使用选择lambda

思路：

选择从小到达的lambda（例子从0.1-》10），针对特定模型，train完后，不用lambda使用cross validation训练数据算cost，cost最小的lambda最合理。

## Learning Curve

使用training set的大小作为横轴，衡量traning set和CV set的表现。

正确的模型，training set和cross verification set的cost应该稳定的随着数据的增大而减少。

high bias的模型，数据越多，train set的cost不会减少，而是趋近一个较大的偏离值。
hign variation的模型，数据越多，train set的cost随着数据增多而变大稳定，但是cv set的值偏离较大。


## Summary
Our decision process can be broken down as follows:

* Getting more training examples: Fixes high variance
* Trying smaller sets of features: Fixes high variance
* Adding features: Fixes high bias
* Adding polynomial features: Fixes high bias
* Decreasing λ: Fixes high bias
* Increasing λ: Fixes high variance.

Diagnosing Neural Networks

* A neural network with fewer parameters is prone to underfitting. It is also computationally cheaper.
A large neural network with more parameters is prone to overfitting. It is also computationally expensive. In this case you can use regularization (increase λ) to address the overfitting.
* Using a single hidden layer is a good starting default. You can train your neural network on a number of hidden layers using your cross validation set. You can then select the one that performs best.


Model Complexity Effects:

* Lower-order polynomials (low model complexity) have high bias and low variance. In this case, the model fits poorly consistently.

* Higher-order polynomials (high model complexity) fit the training data extremely well and the test data extremely poorly. These have low bias on the training data, but very high variance.

In reality, we would want to choose a model somewhere in between, that can generalize well but also fits the data reasonably well.
