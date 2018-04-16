---
title: 《量化投资：数据挖掘技术与实践(MatLab版)》读书笔记第7章：分类方法
postslug: quantitative_investment7
date: 2017-10-12
category: 量化交易
tags: 量化分析

---

 分类算法通过对已知类别训练集的分析，从中发现分类规则，以此预测新数据的类别。

<!-- more -->

[《量化投资：数据挖掘技术与实践(MatLab版)》读书笔记目录]({filename}quantitative_investment_index.md)

分类算法通过对已知类别训练集的分析，从中发现分类规则，以此预测新数据的类别。

首先选择一个分类算法，然后用训练集(Training Set)进行训练，归纳出学习模型。
将新模型用测试集进行验证，验证通过的模型就可以用于预测了。

分类算法有很多，常用的有以下几种：

- K-近邻（KNN）
- 贝叶斯分类
- 神经网络
- 逻辑斯蒂（Logistic）
- 判别分析
- 支持向量机（SVM）
- 决策树

# K-近邻（K-Nearest Neighbors, KNN）

K-近邻算法，把每个样例看做d维空间上的一个点, d是属性的个数。

对于给定的样例，取d维空间中预期距离最近的K个训练样例，取这K个训练样例所标记的类别中的多数者，
作为该给定样例的类别。

KNN算法的有效性依赖于K的选取。K太小，预测不稳定；K太大，容易误判。
从原理上来说，数据的有效参数个数约等于 n/K, 其中 n 是训练集中的样本个数。
实际操作中，需要通过反复实验来确定分类误差率最小的K值。

KNN算法的优点是可以较好的避免样本不平衡问题；对于分类交叉/重叠较多的情况也更适用。

缺点是计算量大，解决办法是剔除作用不大的训练样本，或者对样本进行分层分类。

# 贝叶斯分类

贝叶斯(Bayes)分类是一类以贝叶斯定理为基础的基于概率统计知识的分类算法的总称。

贝叶斯定理：事件A在事件B(发生)的条件下的概率，与事件B在事件A的条件下的概率不一样，
但是二者有确定的关系：

$P(Y|X)=\frac{P(X|Y) P(Y)}{P(X)}$

其中：

- P(Y=y|X=x) 后验概率， 是在 X=x 的条件下， Y=y 发生的概率
- P(Y): 先验概率
- P(X|Y): 条件概率
- P(X): 证据

贝叶斯定理可以用 先验概率、条件概率和证据，计算出后验概率。

贝叶斯分类中最简单的是朴素贝叶斯分类。其思想是：对于给出的待分类项，求解在此项出现的条件下，
各个类别出现的概率。概率最大的概率作为待分类项的类别。

朴素贝叶斯(NB)分类是最优秀的分类器之一，基于类条件独立假设(朴素假设)：

   给定类节点(变量)后，各属性节点(变量)之间互相独立。

根据朴素假设，则有：

$P(X|{C}_{i})= \prod_{k=1}^{m}P({X}_{k}|{C}_{i})$

其中，条件概率$P(X1|{C}_{i}) , P(X2|{C}_{i}) ... P(Xn|{C}_{i})$
可以从训练集数据中求得。于是可以计算出X属于每一个类别 Ci 的概率
$P(X|{C}_{i}) P({C}_{i})$

从上述原理可以看出，朴素贝叶斯分类的三个步骤：

1. 准备工作

   根据具体情况确定特征属性，并对每个特征属性进行划分，人工分类，形成训练样本集合。

   分类器的质量，很大程度由特征属性、特征属性划分、训练样本质量绝对。

2. 分类器训练

   计算每个类别在训练样本中出现的频度， 以及每个特征属性划分对每个类别的条件概率估计，并记录结果。

3. 应用

   输入待分类项，输出待分类项与类别的映射关系。


朴素贝叶斯算法成立的前提是各属性相互独立。其方法简单，准确率高，速度快，可以与决策树和神经网络媲美。

朴素贝叶斯分类能够适应孤立的噪声点，也可以处理属性值遗漏的问题。

实际情况中，往往独立性假设不成立，其准确性会下降。此时可以使用一些改进算法，比如
TAN(Tree Augmented Bayes Network)算法， 贝叶斯网络分类器(Bayesian Network Classifier, BNC)等。


# 神经网络

人工神经网络(Artificial Neural Networks, ANN)是一种应用类似于大脑神经突触联接的结构进行
信息处理的属性模型。通过模拟脑细胞在同一个脉冲反复刺激下能够改变神经元之间的神经键连接强度的特性
进行学习。

ANN由一组相互连接的节点和有向链构成。其结构单元是感知器(perceptron)。每个感知器有多个输入节点
和一个输出节点。感知器对输入节点进行加权，计算输出信号。这个权重就是用来模拟神经元之间的连接强度。

对感知器的训练，就是通过输入数据不断调整链的权重，直到能拟合训练数据。

用公式表示感知器的模型如下：

$\hat{y}=sign(wx-t)$

其中， w是权值向量， x是输入向量， t是偏置因子。

用于分类的神经网络模型常见的有：

- BP(Back Propagation)神经网络
- RBF网络
- Hopfield网络
- 自组织特征映射神经网络
- 学习矢量化神经网络

目前应用较多的时BP神经网络。神经网络的主要缺点是收敛速度慢，计算量大，训练时间长，结果不可解释。

# 逻辑斯蒂（Logistic）

逻辑斯蒂分类，其实就是前面的[逻辑斯蒂回归]({filename}quantitative_investment6.md#logistichui-gui)

# 判别分析

判别分析(Discriminant Analysis, DA), 根据案例的分组变量和特征变量，确定分组变量和特征变量之间的
数量关系，以此建立判别函数(Discriminant Function)。然后利用判别函数对新案例进行分类。

判别分析的假设条件：

- 每个判别变量不能是其他判别变量的线性组合
- 每组案例的协方差矩阵相等
- 各判别变量之间具有多元正态分布，即每个变量对于其他所有变量的固定值具有正态分布

判别函数为以下线性函数关系：

$y={b}_{0} + {b}_{1} {x}_{1} + ... + {b}_{k} {x}_{k}$

其中：

- y 是判别值(Discriminant Score)
-${x}_{i}$为各个判别变量
-${b}_{i}$为判别系数(Discriminant Coefficient or Weight)

判别模型的各个判别变量可以看做 k 维空间，每个案例是空间中的一点。相同分类的点聚集成一组，
该组的中心点用该组中各个案例的平均值代表。

# 支持向量机（SVM）

支持向量机(Support Vector Machine, SVM)，是建立在统计学习理论基础上的机器学习方法，
属于有监督学习。

SVM 在多维空间中构建一个超平面，使得这个超平面能够划分训练数据中的两类节点，并且每一个点
到这个超平面的距离足够大，则这个超平面可以作为分类依据。

这个超平面叫做决策边界，每个数据点垂直指向这个平面的向量叫做支持向量。

对于多类划分，需要构建多个超平面。

对于线性SVM，假设训练样本集$\{({x}_{i},{y}_{i}), i=1,2,...,n \}$有两个类别:
y=1 和 y=-1 。

如果存在分类超平面:

${w}^{T} x + b = 0$

能够将样本正确分为两类(相同类别的样本都落在分类超平面的同一侧)，则称该样本集是线性可分的，即满足：

$\begin{cases}
 & \text{ if {w}^{T} {x}_{i} + b \geq 1 } {y}_{i} = 1  \\
 & \text{ if {w}^{T} {x}_{i} + b \leq -1 } {y}_{i} = -1
\end{cases}$

此时${w}^{T} {x}_{i} + b =1$和${w}^{T} {x}_{i} + b =-1$
这两个平行的平面称为该分类问题的边界超平面。

SVM要求解距离最大 (距离的倒数最小) 的两个边界平面，即最佳超平面。即：

$
min: \frac{1}{2} \begin{Vmatrix} w \end{Vmatrix} = \frac{1}{2} \sqrt{ {w}^{T} w } \\
s.t. {y}_{i} ({w}^{T} {x}_{i} + b )  \geq 1 , i=1,2,...,n
$

这是一个二次优化问题 —— 目标函数是二次的，约束条件是线性的。可以用拉格朗日变换求解。

（过程：略）


对于线性不可分隔的情况，可以使用核函数，将其映射到线性可分的空间再进行求解。


# 决策树

# 分类的评判

# 应用实例：分类选股法

# 延伸阅读：其他分类方法

# 本章小结


[《量化投资：数据挖掘技术与实践(MatLab版)》读书笔记目录]({filename}quantitative_investment_index.md)