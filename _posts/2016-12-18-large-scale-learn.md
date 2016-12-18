---
layout: post
title: 大规模机器学习
date: 2016-12-18
categories: blog
tags: [机器学习]
description: 大规模机器学习
---

如果我们有一个低方差的模型,增加数据集的规模可以帮助你获得更好的结果。我们应 该怎样应对一个有 100 万条记录的训练集?        
以线性回归模型为例,每一次梯度下降迭代,我们都需要计算训练集的误差的平方和, 如果我们的学习算法需要有 20 次迭代,这便已经是非常大的计算代价。    
首先应该做的事是去检查一个这么大规模的训练集是否真的必要,也许我们只用 1000 个训练集也能获得较好的效果,我们可以绘制学习曲线来帮助判断。  

**随机梯度下降法**      
![](https://raw.githubusercontent.com/whuhan2013/myImage/master/machineLearning/class12/p1.png)  


随机梯度下降算法在每一次计算之后便更新参数 θ,而不需要首先将所有的训练集求和, 在梯度下降算法还没有完成一次迭代时,随机梯度下降算法便已经走出了很远。但是这样的 算法存在的问题是,不是每一步都是朝着”正确”的方向迈出的。因此算法虽然会逐渐走向全 局最小值的位置,但是可能无法站到那个最小值的那一点,而是在最小值点附近徘徊。

![](https://raw.githubusercontent.com/whuhan2013/myImage/master/machineLearning/class12/p2.png)  

**小批量梯度下降**      
小批量梯度下降算法是介于批量梯度下降算法和随机梯度下降算法之间的算法,每计算 常数 b 次训练实例,便更新一次参数 θ。      

![](https://raw.githubusercontent.com/whuhan2013/myImage/master/machineLearning/class12/p3.png)  
通常我们会令 b 在 2-100 之间。这样做的好处在于,我们可以用向量化的方式来循环 b 个训练实例,如果我们用的线性代数函数库比较好,能够支持平行处理,那么算法的总体 表现将不受影响(与随机梯度下降相同)。

**随机梯度下降收敛**     
