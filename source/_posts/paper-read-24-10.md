---
title: 10月论文阅读
tags: paper
categories:
  - note
excerpt: 论文阅读小记
published: false
date: 2024-10-17 10:27:08
---

# 10月论文阅读小记

[TOC]

## Learning or Self-aligning? Rethinking Instruction Fine-tuning.

- 24年
- 指令微调

[学习还是自我对齐 ? 关于指令微调的内在机制的探究 – ICIP站点](https://www.icip.org.cn/zh/2024/03/02/学习还是自我对齐-关于指令微调的内在机制的探究/)

### 内容

**对于指令微调而言，学习与模型参数知识不一致的世界知识无法带来增益，甚至会造成额外的损害。**

使用与模型内部参数知识完全一致的**指令**数据并不总能取得最优性能。aligning is not we all need

**有效指令微调的本质在于完成行为模式转换的同时，保持指令微调前后模型参数知识的一致性。**

### 思考

怎样才能更好的完成行为模式的转换？

## Self-Play Fine-Tuning Converts Weak Language Models to Strong Language Models.

- 24年
- 自监督微调

### 涉及概念

#### DPO

在深度学习中，DPO（Direct Preference Optimization）是一种用于优化语言模型以使其输出更符合人类偏好的方法。DPO依赖于人类提供的偏好数据，这些数据通常是以**比较形式**存在的，例如，人类判断哪一条模型生成的文本更符合他们的期望。DPO的核心思想是直接利用人类的偏好数据来优化模型的参数，而不是通过传统的强化学习（RL）或基于人类反馈的强化学习（RLHF）方法。

### 内容

大概是在说LLM既当生成器又当判别器

生成器：尽可能生成和人类类似的输出

判别器：判断一段输出是人类生成的还是LLM生成的

### 思考

感觉这样做只能让模型表现接近人类而不是超越人类

能不能把GAN的那套搬过来？

## Principle-Driven Self-Alignment of Language Models from Scratch with Minimal Human Supervision

- 23年
- 指令微调

想要更少的依赖人工标注的数据

### 涉及概念

#### Self-Instruct

[Self-Instruct 论文解读：利用大模型自己给自己生成指令数据，指令数据自动生成_模型训练 什么叫 指令数据-CSDN博客](https://blog.csdn.net/qq_40491305/article/details/131432186)

> 大白话点讲，就是
>
> 1. **大模型自己遵循一套流程来生成数据**，
> 2. **再用这些生成的数据来指令微调自己**
> 3. 从而提高模型自己的能力。

### 内容

[IBM也下场LLM了，自对齐、高效率的单峰驼Dromedary来了 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/627643379)

[Principle-Driven Self-Alignment of Language Models from Scratch with Minimal Human Supervision - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/628431087)

主要是更大规模的Self-Instruct，然后再进行上下文蒸馏

### 思考

似乎就只是把规模做大了

## Large Language Models are Superpositions of All Characters: Attaining Arbitrary Role-play via Self-Alignment

- 24年
- 角色扮演

[论文阅读 Large Language Models are Superpositions of All Characters - I am LiHua](https://iamlihua.github.io/2024/10/17/large-language-models-are-superpositions-of-all-characters/)

## Self-Rewarding Language Models

- 24年
- 微调

### 内容

[Self-Rewarding Language Models 总结版 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/679297230)

[2024年1月19日Arxiv最热NLP大模型论文：Self-Rewarding Language Models-CSDN博客](https://blog.csdn.net/xixiaoyaoww/article/details/135704504)

和**RLHF**还有**Self-Instruct**比较像，但是它是训练的模型和奖励模型是同时训练的而不是冻结奖励模型

使用的训练数据是偏好对

### 思考

和**Learning or Self-aligning? Rethinking Instruction Fine-tuning.**这篇是不是可以在一起看，模型觉得好的不一定是真正好的。

能不能加入一些对齐的思路？
