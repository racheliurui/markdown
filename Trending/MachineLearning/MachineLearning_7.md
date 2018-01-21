title: Machine Learning - Week 7
mathjax: true
date: 2018-01-13 18:39:16
tags:
- Machine Learning
- SVM
- Large Margin Classifiers
---


# SVM


# Large Margin Classifiers

SVM 又叫Large Margin Classifier。

概念：
Margin of SVM
SVM叫Large Margin Classifier因为这种算法找出来的参数对数据进行区分的时候会找到最大的Margin处才划线。

C跟lambda的值的意义是相反的。C约等于1/lambda。所以，C取很大的值相当于lambda取很小的值，这时候，模型会尽量fit数据（可能会overfit）。反之，C取很小的值的时候，类似于我们把lambda取很大的值一样， 这时候，数据有混合的时候，模型参数会进行忽略那些极个别的数据。


# Kernel


Kernel is a similarity function. 在坐标图上， 它 体现的是一个点到周边feature点的距离比例（结果是0-1， 0代表相似度0，或者说很远，1代表很相似）。
课程中使用的function是Kernel function的一种，叫做高斯kernel（Gaussian Kernel）


SVM 中的C 和 sigma

C 跟lambda相反; 一个大lambda是防止overfit的，那么一个小的C也是一样的效果。所以C越小越容易overfit，越viriant，C越大越容易bias
如果采用高斯kernel，则需要选择sigma

SVM也是用结果logistic regression一样的问题。
