---
title: best practice for code review
date: 2017-11-15 13:51:59
tags:
	- code review
	- code
---

上一篇文章写了[如何做好design](https://acceml.github.io/2017/11/10/%E5%A6%82%E4%BD%95%E5%81%9A%E5%A5%BD%E7%9A%84design/)，其实也是一个如何指导新人，做design review的方法论。由于tech leader是一个有代码洁癖的人，组内的代码互相review一直都特别严格，经常一份小代码提了数十个comments。这里同样总结一些方法论，如何做好code review。

# None-goal

在谈code review之前，我觉得有必要强制一下code style。避免code review的时间浪费在空格、命名规范等不痛不痒的事情上。由于笔者主语言是java，这里推荐三个插件或包：

- lombok。`@Slf4j`、`@Getter` 、`@Setter` 注解可以达到简化代码的目的;
- checkstyle。强烈建议加上这个插件，统一团队代码风格，如果出现magic number、illegal blank、参数过多，代码超过多长等的时候直接compile failure;
- findbug。可以发现一些潜在的bug。 

此外，一些基本的命名风格，如DTO、DAO的命名等，可参考[阿里巴巴java开发指南终极版本](https://github.com/alibaba/p3c)。

code review 不应该包括：设计模式，架构变动这些东西，这些东西应该是在design review上面，而不是code review可以说清楚的。

code review不积极，如何鼓励大家code review、业务发展太快不能review等问题不在本文讨论之列。

# Goal

## 依赖

- 包的是否有SNAPSHOT
- 引入新的包是不是和已有功能是不是能承载了，比如判断一个Map、List是不是空，就有spring、apache、guava等包，团队尽量保持一致；json的包那么多，使用一个就好。

## style&&clean

- 遍历的方式。如果用lamda就都用lamda，不要一边lamda一边又for()，当然复杂情况ok。

- debug日志代码不要出现在master branch。

- for循环中break,continue的方式。比如通常的循环都可以先写break，再写continue。

  ```java
  for (CommonAd commonAd : candidateApps) {
    	if(xxx){
        break;
    	}
    	if(xxx){
        continue;
    	}
      xxx;''
  }
  ```

- 各种if之间如果是逻辑段要空行.

- 禁止出现大段代码相同.

- 不要提前new Object。

  ```java
  for(){
    App app = new App();//这里就可能被continue掉。
    if(xxx){
      continue;
    }
  }
  ```

  ​

- 参数分行了就一行一个参数。

- lamda转行的时候保持风格统一，通常是这样子。

  ```java
  list.stream()
    .filter()
    .map()
    .Collector()
  ```

- ​

## 命名&&声明

- 无用的变量。有的时候代码改动较多的时候，经常会`@Autowired` 一个bean结果没用到。
- 静态变量的声明。是不是有重复、或者可以提高到公用的Utils。
- 变量的命名要一目了然。不要出现index, i,j,k等变量，所有的变量命名都要有意义。
- magic string。magic number可以用checkstyle检查出来，但是有的变量也必须要声明的，不然后面读代码会有一种想死的冲动。
- 注释的相关性和清晰度以及是不是需要注释。

## 数据结构&&API

- Map里面只放所需要的变量，不要随便就addAll()。

- 不要随便new xxxList。用Collections.emptyList()替代，存储做了优化。

- 使用set还是sublist；使用linkedList还是ArrayList诸如此类。

- 有没有现成的api。

- 保持类的单一职责原则。避免过度耦合不可维护。

- 避免在运行时计算，比如在都是变量的话，恰好这两个变量相关，提前计算好，不要在执行代码的时候计算。

  ```java
      private static final long A = 5;
      private static final long A_B = A / 2;
  ```

- 方法的单一职责，要么拆方法，要么换名字。保大的还是保小的你总要选一个(跑偏了...)

## 监控&&日志

- counter不要随便。该加counter的时候要加，线上业务是不是关心这个请求。
- 离线日志只在出入口打，除非特殊需要。

# reference

* [大家的公司的code review都是怎么做的？遇到过什么问题么？](https://www.zhihu.com/question/41089988)

