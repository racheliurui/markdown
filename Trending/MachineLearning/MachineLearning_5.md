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


## 算法实现细节

### Rolling parameters

为了使用跟之前一样的方式调用fminunc()求最优的theta，不方便传入多维数组。
这里引入unroll 的parameter的概念。因为对于神经网络模型，把多维theta matrix给unroll成一个大vector并不影响计算。算出来的结果再reshape回来即可。

```Octave
thetaVector = [ Theta1(:); Theta2(:); Theta3(:); ]
deltaVector = [ D1(:); D2(:); D3(:) ]
```

```Octave
Theta1 = reshape(thetaVector(1:110),10,11)
Theta2 = reshape(thetaVector(111:220),10,11)
Theta3 = reshape(thetaVector(221:231),1,11)
```

总结：
unroll theta的matrix得到大vector调用fminunc求theta
reshape得到theta用来计算D 和 J(theta)
unroll D得到DVector用来计算gradientVector（？？？）

### Gradient Checking

在使用fp 和 bp的过程中用来检查实现是否正确。

公式如下：
{% math %}
\dfrac{\partial}{\partial\Theta}J(\Theta) \approx \dfrac{J(\Theta + \epsilon) - J(\Theta - \epsilon)}{2\epsilon}
{% endmath %}

对于多维theta的神经网络，该公式表示为（即为每个theta求一次numerical derivative ,注意这样的验证超级消耗计算资源，所以该算法值用来验证算法BP实现的的正确性，一旦验证就不需要反复调用该公式做验证。
{% math %}
\dfrac{\partial}{\partial\Theta_j}J(\Theta) \approx \dfrac{J(\Theta_1, \dots, \Theta_j + \epsilon, \dots, \Theta_n) - J(\Theta_1, \dots, \Theta_j - \epsilon, \dots, \Theta_n)}{2\epsilon}
{% endmath %}

其中
{% math %}
{\epsilon = 10^{-4}}
{% endmath %}

```Octave
epsilon = 1e-4;
for i = 1:n,
  thetaPlus = theta;
  thetaPlus(i) += epsilon;
  thetaMinus = theta;
  thetaMinus(i) -= epsilon;
  gradApprox(i) = (J(thetaPlus) - J(thetaMinus))/(2*epsilon)
end;
```
### Random Initiation

theta初始值设为全0，对于logical gradient 算法管用，但是对于neron network不行，最终会形成死循环但找不到合理参数。
我们需要做random initiation， 而且必须打破symmetry （symmetry breaking），最后得到的随机数参数应该在正负EPSILON之间。

If the dimensions of Theta1 is 10x11, Theta2 is 10x11 and Theta3 is 1x11.(这里的INIT_EPSILON是Ocatave保留常量)

```Octave
Theta1 = rand(10,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
Theta2 = rand(10,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
Theta3 = rand(1,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
```

# Summary

## 选择Neron Network的architecture

1) input个数根据feature个数选择
2) output个数根据classification个数选择
3) 默认一层hidden layer，当然越多越好，但是运算量越大
4) 每层hidden layer的unit数选择相同个数。

## Train Neron Network

1) Randomly initialize the weights

2) Implement forward propagation to get hΘ(x(i)) for any x(i)

3) Implement the cost function

4) Implement backpropagation to compute partial derivatives

5) Use gradient checking to confirm that your backpropagation works. Then disable gradient checking.

6) Use gradient descent or a built-in optimization function to minimize the cost function with the weights in theta.


```octave
for i = 1:m,
   Perform forward propagation and backpropagation using example (x(i),y(i))
   (Get activations a(l) and delta terms d(l) for l = 2,...,L
```

Keep in mind that J(Θ) is not convex and thus we can end up in a local minimum instead.

# Neron Network 的应用案例

自动驾驶，
3层网络，2分钟就学会了驾驶员对图像的判断处理

怎么判断？
