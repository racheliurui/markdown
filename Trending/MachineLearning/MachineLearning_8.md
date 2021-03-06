title: Machine Learning - Week 8
mathjax: true
date: 2018-01-20 15:05:16
tags:
- Machine Learning
- Unsupervised Learning
---


# Unsupervised learning overview

栗子：

market segment； 社交网络； 机器群集； 天文数据

## K-means 算法

给一堆数据，请把它们分成K类。怎么做？

1） 随机得到K个点。

loop直到K的值不再变化{

  2） 计算每个数据到K的点的距离，如果数据组i到K（j）的距离最小，就认为数据i属于K（j）这个组。
      数据全部分类完之后，调整K个点的值，每个K直接赋值为当前被分到该类的数据的平均值。

}

### K-means的Optimization Objective

supervised ML有cost function； 这种unsupervised很难有正确答案来衡量。那么如何证明做得好不好呢？

K means最终的目标是： （（每个点到自己所属的参照点的距离）的平方）的和最小。

### 如何random初始化Cluster Centroid

idea： run k-means 很多次，每次都random initialize centoid，得到cluster后，算cost function，比较用最小cost的

这样可以避免local optism

### 如何选择cluster number

没有正确答案。

* Elbow Method：根据分组的不同，cost function随着cluster的数字变化（luckily会）呈现elbow的曲线（大多情况下没有这种图像）
* 根据需要来决定分组数目。 比如，T恤尺码分组（SML or xs SML xl）

### 使用k-means进行图像压缩的原理

假设：原图是256色（每个pixel使用三组数据表示红黄蓝的梯度颜色，每组数据是8位二进制（0-256））。
目标：选择16色来代表原图（每个pixel使用三组数据，每组数据是4位二进制（0-16））。

使用k-means： 将原图的每个pixel看作一组数据，使用k means尝试将其分组到16色的分组中。 这里分组的目标确定了。
原先的每个pixel是256*3个feature, 新的目标映射每个pixel是16*3个feature。所以我们要尝试将数据分组为256/16=16组。

随机选择16个颜色（16*3）
计算每个pixel到这些颜色的距离，找出centroids，然后重新找k
直到k不再变化。
然后将原先像素点属于的k算出来取代原先的pixel

---》pixel个数不变，颜色维度变小。原图像（256*3*pixel数目），新的图像（16*3*pixel数目），压缩了16倍。


# Motivation

## 数据压缩

   2D -》 1D （当数据都在一条线上）
   3D -》 2D （当数据都在一个平面上）

   数据visulization依赖于数据压缩，因为一般来说显示数据都是2D或者3D

## Principal Component Analysis - PCA


* Linner Regression和PCA的区别

Linner Regression是找每个点到目标直线的沿着坐标轴的距离。PCA则是找每个点project到目标直线的距离。

PCA不是Linner Regression

### PCA的算法

* 先处理数据： feature normalization 和 mean normalization
* 应用PCA算法将数据从n维降低到k维：

先算sigma值，以下是如果x是一个vector，从1到n loop所有的vector的情况。
{% math %}
\begin{align*}
Sigma=\frac{1}{m}\displaystyle\sum\limits_{i=1}^n (x ^\left(i\right))(x ^\left(i\right))^T
\end{align*}
{% endmath %}

当X来代表所有数据的时候，
{% math %}
\begin{align*}
Sigma=\frac{1}{m}X^TX
\end{align*}
{% endmath %}

使用SVD (singlar value decomposition)算法，带入sigma。

{% math %}
\begin{align*}
[u,s,v] = SVD (Sigma)
\end{align*}
{% endmath %}

返回的U是一个n*n的matrix。取前面k列，即得到n*k的matrix，这个matrix叫{% math %}u_{reduce}{% endmath %}.


对于单个x， {% math %}u_{reduce})^Tx{% endmath %}即可得到reduce的z。
对于一组X数据， 则直接进行矩阵求值：

```octave
U_reduce = U(:,1:K);
Z = X * U_reduce;
```

### 从压缩的数据反向得到原始数据
n*k的{% math %}u_{reduce}{% endmath %} 乘以 k*1 的z可以得到大致的最初n*1的x vector

{% math %}
\begin{align*}
x_{approx}=u_{reduce}*z
\end{align*}
{% endmath %}

### 如何选择压缩维度k

“99% of variance is retained”

这句话的意思是，经过压缩后，

{% math %}
\begin{align*}
\frac{  \frac{1}{m}\displaystyle\sum\limits_{i=1}^m \lVert x ^\left(i\right)-x_{approx} ^\left(i\right)\rVert ^2} {\frac{1}{m}\displaystyle\sum\limits_{i=1}^m \lVert x ^\left(i\right)\rVert ^2} \leqslant 0.01
\end{align*}
{% endmath %}

根据这个参数指导寻找最优的k值

* Option1， k=1开始，逐渐放大k，直到上述的公式成立
* Option2， 使用SVD (singlar value decomposition)算法，返回值的s是一个n*n的diagonal matrix （除了左上到右下的对角线，其它部分为0）。

{% math %}
\begin{align*}
[u,s,v] = SVD (Sigma)
\end{align*}
{% endmath %}


根据SVD的结果判断以下是否成立，如果不成立，则k不满足要求。
{% math %}
\begin{align*}
\frac{\sum\limits_{i=1}^k s_{ii}}{ \sum\limits_{i=1}^n s_{ii} }\geq0.99
\end{align*}
{% endmath %}

* PCA 算法使用时应该只用于training data set，而不是cv或者test data set。

#### PCA的意义

* 压缩数据，减少存储，加速算法
* 压缩数据，方便visualization展示数据（人类肉眼智能观察最多3D的数据）

错误的使用场景：

* 错误1: 使用PCA用来做防止overfitting，但是很容易丢失数据中的有用信息； instead还是使用regularization才是正确的做法。
* 错误2: PCA作为默认步骤处理数据。只有正常的分析不work时候，才考虑使用PCA进行数据压缩。
