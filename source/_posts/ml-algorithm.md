---
title: 机器学习常见领域记录
tags: algorithm
categories:
  - learn
excerpt: 记录一下机器学习的各种领域
published: true
date: 2024-10-07 19:19:18
---

# 机器学习记录

之前做过一篇偏课内的笔记 [机器学习复习 - I am LiHua](https://iamlihua.github.io/2024/06/09/ml-learn/)

## 半监督学习&自监督学习

[一文看懂半监督学习(Semi-supervised Learning)和自监督学习(Self-Supervised Learning)](https://blog.csdn.net/qq_44015059/article/details/106448533)

> 将大量的无类标签的样例加入到有限的有类标签的样本中一起训练来进行学习，期望能对学习性能起到改进的作用

自监督学习是一种无监督学习的方法

## 对比学习

[【深度学习：（Contrastive Learning） 对比学习】深入浅出讲解对比学习](https://blog.csdn.net/jcfszxc/article/details/135381129)

> 在学习到的嵌入空间中，相似的实例应靠得更近，而不相似的实例应离得更远

对比学习是自监督学习的一个子集

## 元学习

[一文入门元学习（Meta-Learning）（附代码）](https://zhuanlan.zhihu.com/p/136975128)

<img src="https://pic4.zhimg.com/v2-2155d09e7227f572140b5e01c420daf7.webp" alt="img" style="zoom:50%;" />

> 二者的目的都是找一个Function，只是两个Function的功能不同，要做的事情不一样。机器学习中的Function直接作用于特征和标签，去寻找特征与标签之间的关联；而元学习中的Function是用于寻找新的f，新的f才会应用于具体的任务。

有点像 普通方程和微分方程的区别

## 小样本学习

[【学习笔记】小样本学习（Few-shot Learning）_小样本训练](https://blog.csdn.net/zhaohongfei_358/article/details/124057980)

[小样本学习（Few-shot Learning）](https://zhuanlan.zhihu.com/p/61215293)

> 可以学习一个相似度函数sim(x,x') 来判定样本x和x'的相似度，相似度越高，表示这两个样本越可能是同一个类别。例如，可以通过一个很大的数据集学习出一个相似度函数，然后用该函数进行预测。

元学习可以用于小样本学习

## 迁移学习

[迁移学习概述（Transfer Learning）](https://blog.csdn.net/dakenz/article/details/85954548)

> 将某个领域或任务上学习到的知识或模式应用到不同但相关的领域或问题中。

会看源数据和目标数据的分布差别/使用共享参数这样的方法

## 大模型的微调

[【大模型微调】一文掌握7种大模型微调的方法](https://community.modelscope.cn/66f907112db35d1195f223b8.html)

> 文章探讨了大型模型微调的技术手段，包括全面微调和参数高效微调（PEFT），并详细介绍了PEFT中的各种方法，如LoRA、QLoRA、适配器调整、前缀调整、提示调整、P-Tuning及P-Tuning v2等。

大模型的微调是迁移学习的一种形式

