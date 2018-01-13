title: Machine Learning - Week 6
mathjax: true
date: 2018-01-09 21:43:11
tags:
- Machine Learning
- Machine Learning Methodology
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


# 下一步的工作


举个邮件spam的例子：

* Honeypot project --》 负面的例子。 为了解决邮件spam的问题，突发奇想注册专门的邮箱收集大量spam邮件来进行数据分析。虽然spam问题确实需要大量数据，但是这样做并不能保证成功。
* 通常spam分析会有1w到5w的word作为feature，可能会通过邮件分析top常用词的方式。
* 很复杂的情况会出现：区分大小写吗？区分单复数吗？错误的拼写如何应对？（w4tch，代表watch，但是正常不会被算作过滤词，因为拼写错误）
* 加入复杂的feature，对于邮件的header区域的信息进行分析。



## Error Analysis


* 算法从简单的开始，迅速实现，使用cv数据测试，验证
* 观察learning curve决定是否需要更多数据？ 更多feature？
* Error Analysis --- 以spam为例，把算法归类错的训练数据拿出来进行分析。
  * 对错误进行分类，比如，如果fishing email这块做得最差，那么改进算法应该focus在这一块
  * 进行numerical evaluation。加入steming和不用stemming；区分大小写和不区分大小写； 哪种方式效果好？

## 处理skewed data

什么是skewed data？
典型场景： 癌症分类算法，实际数据0.5%的总人数是positive，算法error比例是1%。这种情况下，hardcode result=0的error比例将为是0.5%，这个结果看起来都会比算法的error比例1%小。那么怎么正确衡量算法的准确度呢？

这里引入两个概念：Pricision和recall
在算法中，rare case的情况我们标示为1.
Pricision：在所有算法预测结果为1的训练数据中，有多少是实际是1？
Recall： 在所有实际是1的训练数据中，有多少被算法预测为1？

上面这两个比例越高说明算法准确度越高。Cheating的算法是无法通过Pricision和Recall的衡量测试的。
例如，上面的cheating算法，recall为0.

### trading off precision和recall

实际算法比较中，如果我们调整threshold（阀值），我们可以看到precision和recall之间的关系。
比如，比例》0.9才算positive，那么precision会很高，但是recall就会变低。反之亦然。

这样的情况，如果有多个算法，recall高的precision低，我们如何比较算法的准确率呢？
首先，（recall+precision）/2 的值用来比较是绝对不行的，参见上面hardcode result=0的算法，那个算法使用平均值衡量的话，会可能脱颖而出。

需要一种算法能够综合考虑precision和recall的表现，这种情况有多种公式可以处理，本课程引入F1的算法。
F1 = 2* P*R（P+R）

基本上就是P和R表现的平均好的才会脱颖而出。

## 使用大量数据

前面讲过，算法不对，数据多也是白搭。
但是对于一些算法，比如AI词汇填空，实际证明了，数据越多，算法越精准。那么怎么衡量数据多好不好呢？

提问： 如果你有这些数据，对于human expert来说，他能不能精准的给出正确答案？if yes， go ahead， 收集越多数据，模型会越精准。if no，说明再多数据也没用。

极端例子：比如只收集的房子的面积数据，如果问地产中介，他也给不出估价。这种情况就说明，数据多了也没用。
