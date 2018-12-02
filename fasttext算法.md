---
title: fasttext算法
date: 2018-05-22 19:47:42
categories: 机器学习
tags:
- 机器学习
- NLP
---

CBOW模型能够根据输入周围n-1个词来预测出这个词本身。CBOW模型的输入是某个词A周围的n个单词的词向量之和，输出是词A本身的词向量。

skip-gram模型能够根据词本身来预测周围有哪些词。而skip-gram模型的输入是词A本身，输出是词A周围的n个单词的词向量(对的，要循环n遍)。

#  N-Gram

## 字粒度

我 来到 达观数据 参观

相应的bigram特征为：我来 来到 到达 达观 观数 数据 据参 参观

相应的trigram特征为：我来到 来到达 到达观 达观数 观数据 数据参 据参观

## 词粒度

相应的bigram特征为：我/来到 来到/达观数据 达观数据/参观

相应的trigram特征为：我/来到/达观数据 来到/达观数据/参观

fasttext是采用字符粒度的.

## word embedding

通俗的翻译可以认为是单词嵌入，就是把X所属空间的单词映射为到Y空间的多维向量，那么该多维向量相当于嵌入到Y所属空间中，一个萝卜一个坑。

word embedding，就是找到一个映射或者函数，生成在一个新的空间上的表达，该表达就是word representation。

[fasttext分类tutorial](https://fasttext.cc/docs/en/supervised-tutorial.html)

