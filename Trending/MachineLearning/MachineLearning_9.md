title: Machine Learning - Week 9
mathjax: true
date: 2018-01-27 12:54:16
tags:
- Machine Learning
- Density Estimation
- Gaussian Distribution
- Anomaly Detection Algorithm
- recommender system
- Collaborative filtering Algorithm
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

{%math%}\thicksim{%endmath%} 读作“distributed as”，N是normal的意思，{%math%}\mu{%endmath%}代表正态分布的最高点投射到横轴的读数，{%math%}\sigma{%endmath%}表示的是正态分布的驼峰的宽度。{%math%}/sigma{%endmath%}又叫standard deviation.

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

## Density Estimation


当数据是n维的时候，我们的p(x)公式变成了：


{%math%}
\begin{align*}
p(x)= \prod_{j=1}^np(x_j;\mu_j,\sigma_j^2)
\end{align*}
{%endmath%}

### Algorithm evaluation

直接使用cv或者test set算正确率不准确，因为stewed data。
使用cv set选择{%math%}\varepsilon{%endmath%}

比如： 10000个正常引擎数据，40个不合格引擎的数据。

推荐，6000个数据用来做算法求正态分布
     2000个正常引擎和20个不合格引擎的数据用来做CV set，选择{%math%}\varepsilon{%endmath%}
     2000个正常引擎和20个不合格引擎的数据用来做test set，用来验证模型正确。

### Anomaly Detection vs Supervised Learning

Anomaly Detection： 特别适合属于Anomaly的数据非常少的情况，即y=0的数据非常多，y=1的数据也就几十个（1-20个）。未来的Anomaly data可能和现有的数据完全不一样。
Supervised Learning： 大量的数据，有positive和negative，未来的negative数据会跟现有的有一定的类似。

典型的对比，Fraud detection： 如果我们还不知道Fraud一般会有什么特征，那么使用Anomaly Detection我们可以找到特别不同寻常的操作；但是如果我们有大量的数据做参考，那么这种Fraud detection就可以使用Supervised Learning来建模了，例如spam letter的算法。

* Anomaly Detection的例子：  Fraud Detection； Manufacturing （例如飞机引擎）； 监控数据中心的机器
* Supervised Learning的例子：Spam letter labeling；Weather Prediction； Cancer Classification

### Choose correct features for Anomaly detection Algorithm

先把数据plot出来

```Octave
% check the data size
size(x)
% plot the data
hist(x)
% adjust the plot display
hist(x,50)
% transform the data, until it looks more gaussian
hist(x.^0.5,50)
hist(x.^0.2,50)
hist(x.^0.1,50)
% define the new feature based on original one
xNew=x.^0.1
```

常用技巧： 使用正态分布找出异常数据，观察异常数据的异常之处，然后定义新的feature出来。
常用技巧： 挑选数据在异常时候能说明问题的，比如监控数据中心的例子中，cpu和网络traffic一般是线性关系，如果使用cpu/网络traffic就可以得到一个feature帮助识别cpu的infinite loop或死锁之类的问题。



## Recommendation System Formulation

### Predicting Movie Rating

使用Liner Regression来预测user对电影的打分。
方法： 创造feature，如action，romantic等，然后为每个movie量化这些feature，最后，根据用户之前的打分来找出用户的打分模型，从而预测用户的打分。

#### Collaborative filtering

中心思想：

先猜测一下电影的feature
Loop
{
已知电影的feature，根据用户打分可以预测/修正用户的打分模型
已知用户打分模型（例如最喜欢action电影，不喜欢romance），可以根据用户打分预测/修正电影的feature参数
}

book store & clothing store
### Collaborative filtering Algorithm

与其loop求值，不如将两组数据结合起来一起看。

cost function是：

{%math%}
\begin{align*}
J(x^{(1)},,,x^{(n_m)},\theta^{(1)},,,\theta^{(n_u)})=\frac{1}{2}\sum_{ (i,j) :r(i,j)=1} (\theta^{(j)})^T
 x^{(i)} - y^{(i,j)} )^2 +\frac{\lambda }{2}\sum_{i=1}^{n_m}\sum_{k=1}^{n}{({x_k}^{(i)} )}^2+\frac{\lambda }{2}\sum_{j=1}^{n_u}\sum_{k=1}^{n}{({\theta_k}^{(j)} )}^2
\end{align*}
{%endmath%}

对应的算法是：
step 1， random 初始化所有的x和theta
step 2， gradient descent

求x，
{%math%}
\begin{align*}
{x_k}^{(i)}:={x_k}^{(i)}-\alpha (\sum_{ j :r(i,j)=1} ((\theta^{(j)})^T
 x^{(i)} - y^{(i,j)} ){\theta_k}^{(j)}  +\lambda {x_k}^{(i)} )
\end{align*}
{%endmath%}


求theta，
{%math%}
\begin{align*}
{\theta_k}^{(j)}:={\theta_k}^{(j)}-\alpha (\sum_{ i :r(i,j)=1} ((\theta^{(j)})^T
 x^{(i)} - y^{(i,j)} ){x_k}^{(i)}  +\lambda {\theta_k}^{(j)} )
\end{align*}
{%endmath%}


注意参数的random初始化，否则会跟Neron network类似的symetric问题。

### low rank matrix factorization

上述算法中的matrix化的表达方式。

预测打分： X*Theta^
找到类似的电影：补公式
{%math%}
\begin{align*}
X = \begin{bmatrix} - & (x^{(1)})^T & - \\ & \vdots & \\ - & (x^{(n_m)} & - \end{bmatrix},\ \Theta = \begin{bmatrix} - & (\theta^{(1)})^T & - \\ & \vdots & \\ - & (\theta^{(n_u)} & - \end{bmatrix}
\end{align*}
{%endmath%}


{%math%}
\begin{align*}
X \Theta^T
\end{align*}
{%endmath%}


### mean normalization 的使用

当用户从来没打分过，根据前面的算法，预估的打分会是全零。
这不合理。
昨晚mean normalization后，未知用户的打分就接近平均分了。
