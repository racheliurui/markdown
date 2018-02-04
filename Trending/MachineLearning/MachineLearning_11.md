title: Machine Learning - Week 11
mathjax: true
date: 2018-02-04 12:17:16
tags:
- Machine Learning
- Photo OCR
- sliding window
- synthetic data
- ceiling analysis
---

# photo OCR (Optical Character Recognition)


__Pipeline__

解决复杂问题的思路。

以photo OCR为例， pipeline为：

图片--》识别文字区域--》文字分离--》单个文字识别

sliding window的使用： 例如一个图片找行人，已知一个算法可以识别一个20*100的方块里有没有人形，我们就可以使用从小到大不同尺寸的比例方块，每次移动一个sliding window的距离，切下来一个方块，调整到算法要求的比例尺寸，进行判断。

类似思路用在OCR上：  

step1， 切方块，找可能的字符区域
step2， “expansion”算法，把字符区域放大，找出text rigion，并根据比例特征划掉干扰区域。
step3， 挑选出来的区域变成透明，其它区域全部遮住，对选出来的区域进行识别。
step4， 1D sliding window找出单个字符
step5， 识别单个字符

## 如何得到大量训练数据

以OCR为例，
real data，从真实图像中切出来的字母块
另外一个重要来源是： synthetic data： 人工合成

比如：
1） 使用字体库加随机背景制造。
2） 单个字体放大切块，进行distortion（变形）处理。


类似的思路用在声音识别中：
用一个标准的人声数据，加入不同的背景噪声，我们可以得到多个训练数据。

注意要不停问自己的问题，
1） 我的模型对吗？
2） 我要多久时间可以得到比现在多10倍的数据？我可以合成一些吗？ 我可以自己做吗？可以外包吗？（crowd source - 例如amazon mechenical turk）

## ceiling analysis

对于复杂问题，pipeline的每个节点都要资源，如何分配？ 使用ceiling analysis

从pipeline 源头开始，整个的训练数据的正确率70%，那么如果第一步的算法，我们直接假定算法准确率100%（将正确数据输入给下一步），最终正确率变成89%，iterate这个步骤（沿着pipeline方向），直到准确率提升到100%。

回过头看，那个步骤提升到100%对最终的准确率提升的最大？
