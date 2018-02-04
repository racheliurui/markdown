title: Machine Learning - Week 10
mathjax: true
date: 2018-02-03 11:14:16
tags:
- Machine Learning
- Stochastic Gradient Descent
- mini-batch Gradient descent
- Map Reduce 
---

# 数据为王

__It's not who has the best algorithm wins, it's who has the most data.__

如何判断数据是否真的为王，以及多少数据就可以称王，可以使用learning curve帮助判断。
复习learning curve：
* 横轴是训练集大小，纵轴是cost，
  * 先使用大小为m1的training set；求解后算cost，再带入CV set求cost，画两个点。
  * 使用大小为m2的训练集。。。
* 当训练集足够大，两条曲线贴近的时候，说明


# 如何解决数据集太大的问题


## Stochastic Gradient Descent
传统Gradient descent 我们叫batch Gradient Descent，就是说算的时候所有数据都放进去。
Stochastic Gradient Descent, 换了一种思路，把数据一组一组取出来，使用当前选中的数据组调整参数，再取下一组数据。


```
1） Randomly Shuffle Training Examples
2)  repeat (1~10){
    for i:=1...m{
    theta:=theta-alfpha(h(x)-y)x;
        }
}
```
如果有300，000，000数据，使用batch Gradient Descent，每次调整参数就需要所有的数据参与计算。但是对于stochastic gradient descent，一次repeat一次全数据参与，最多十次，运气好（同时也是数据多的时候），一次就可以得到很好的结果。
这是为什么这种算法很快的原因。

### stochastic gradient descent converging

如何判断算法在converging？
思路： 每组数据都用当前theta值算一下cost然后再更新theta， 每loop过1000组数据，算一下之前1k组数据cost的平均值。这样每1k组数据和之前1k组数据比较，看cost是不是在降低。
如何看图（横轴是iteration，纵轴是每隔1k的cost）：
1） alpha越小，曲线越平滑。越可能找到global optimization，但是慢
2） 1k看一次cost还能改成例如5k看一次，改的越大，曲线越平滑，但是缺点是要等很久才能再一次评估算法效果
3） 有时候曲线很多杂音，很难看出趋势，这时候可以调整1k到5k，可能趋势就可以看出来了，如果还是看不出，可能算法有误。
4） 如果曲线不下降反而上升，可能alpha太大了。

为了让它converge，我们可以随着学习，逐步降低alpha，例如 alpha=const1/(interationNum+const2)

## mini-batch Gradient Descent

* batch gradient descent : use m examples one iteration
* stochastic gradient descent: use 1 examples one iteration
* mini-batch gradient descent : use b examples one iteration (b is 2~200)

** mini-batch 只有使用vectorized implementation 的时候性能才会比stochastic gradient descent。


# 相关应用

## online learning
类似stochastic gradient descent的思路，不停有新的streaming数据来，不停调整theta

## map reduce
大data set分成多组，并行计算公式中求和的部分，送到汇总节点，做最后一步的计算。

一些算法库，自动嵌入了map reduce功能。只要实现算法，实现的时候就会自动并行。
