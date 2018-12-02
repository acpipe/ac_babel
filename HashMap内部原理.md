---
title: HashMap内部原理
date: 2018-05-16 09:30:06
categories: java
tags:
- java
- 源码
mathjax: true
---

* hash链表实现冲突.
* hash算法
  * 取hashcode。int的有符号数，**-2147483648**到**2147483648**，直接映射hash内存放不下的。
  * 高位运算。利用**搅动函数** 。**混合原始哈希码的高位和低位，以此来加大低位的随机性**。混合后的低位掺杂了高位的部分特征，这样高位的信息也被变相保留下来。
  * 取模运算。做了优化，因为bucket的大小总是 $2^N$, 经过搅动函数之后的hashCode满足：
    * $hashCode \bmod 2^N = hashCode \wedge (2^{N} -1)$ 

![](HashMap内部原理/运算.jpg)

* 扩容机制
  * 冲突大于8，由链表转为红黑树.
  * 扩容的大小为$2^{N}$

![](HashMap内部原理/hashMap-put方法执行流程图.png)

* 线程安全.
  * ConcurrentHashMap线程安全.分段锁实现。

# 资料

* [Java 8系列之重新认识HashMap](https://tech.meituan.com/java-hashmap.html)
* [JDK 源码中 HashMap 的 hash 方法原理是什么？](https://www.zhihu.com/question/20733617/answer/111577937)