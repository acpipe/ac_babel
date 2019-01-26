# 设计模式

* [单例模式](./单例模式.md)


* [建造者模式在日志系统中的应用](./建造者模式在日志系统中的运用.md)
* [策略模式使用的小技巧](策略模式使用的小技巧.md)

# java

## 并发

* [synchronized实现原理](https://blog.csdn.net/javazejian/article/details/72828483)
* [不可不说的Java“锁”事](https://zhuanlan.zhihu.com/p/50098743)
* [ThreadLocal](https://www.zhihu.com/question/23089780/answer/62097840)
* [ThreadLocal2](http://www.jasongj.com/java/threadlocal/)

```java
public class Foo
{
    // SimpleDateFormat is not thread-safe, so give one to each thread
    private static final ThreadLocal<SimpleDateFormat> formatter = new ThreadLocal<SimpleDateFormat>(){
        @Override
        protected SimpleDateFormat initialValue()
        {
            return new SimpleDateFormat("yyyyMMdd HHmm");
        }
    };

    public String formatIt(Date date)
    {
        return formatter.get().format(date);
    }
}
```

```
    private static final ThreadLocal<Map<String, String>> MAPPING = ThreadLocal.withInitial(HashMap::new);

    public static void doBefore(Client client) {
        MAPPING.get().put(A, B);
    }



    public static void doAfter(ClientInfoV3 clientInfoV3) {
         MAPPING.get().get(A);
    }
```





## 源码

* [HashMap内部原理](HashMap内部原理.md)
* [java快速读取文件](java快速读取文件.md)

## jvm

* [GC区域](./GC区域.md)
* [GC算法和分代回收设计](./GC算法和分代回收设计.md)
* [垃圾回收器分类](垃圾回收器分类.md)
* [G1垃圾回收器](G1垃圾回收器.md)
* [G1参数设置](G1参数设置.md)
* [java类加载机制](java类加载机制.md)
* [jvm命令使用](jvm命令使用.md)
* [从实际案例聊聊Java应用的GC优化](https://tech.meituan.com/jvm_optimize.html)
  * young gc 和 full gc 频繁，调大新生代，young gc 回收时间 = 扫描 + 复制，复制的变少，young gc 回收次数变少，且回收时间不会长。young gc 次数少，相应的full gc 也会变少。
  * 请求高峰发生gc，可用性下降
    * CMSMaxAbortablePrecleanTime 调大，abortable preclean Time 会进行young gc，减少remark 的对象
  * full gc 频繁，老年代不够，动态扩容引起
    * java8 去除了老年代，引入元空间
* G1 参数  
  * [去掉StringDedup](https://stackoverflow.com/questions/42080648/why-when-you-would-not-want-to-have-java-8-usestringdeduplication-enabled-in-jvm)

# Spring

* [Spring-Aop](Spring-Aop.md)

# 代码&&架构&&设计

* [问题定位经验](问题定位经验.md)
* [Best Practice For Code Review](best-practice-for-code-review.md)
* [如何做好的design](如何做好的design.md)
* [分布式锁的几种实现方式](分布式锁的几种实现方式.md)
* [一致性hash](一致性hash.md)
* [基于bool表达式的广告索引](基于bool表达式的广告索引.md)
* [实验平台设计](http://ningg.top/experiment-series-flow-router/)
* 广告索引
  * bool 表达式，索引的量级最大的时候就是广告的数量
  * 更新索引的一致性
    * 用一个时间戳，大于某个时间戳的才更新过来
    * 重启机子的时候
* redis 双写
* 美团高可用系统
  * 限流
  * 熔断
  * 压测

# Netty

* [什么是 TCP 拆、粘包？如何解决？](https://juejin.im/post/5b67902f6fb9a04fc67c1a24)
  * 设置定长消息,例如每个报文的大小固定长度为200字节
  * 通过特殊字符,例如通过回车换行进行分割
  * 将消息分为消息头和消息体,消息头包含消息总长度
* [Netty线程模型](https://crossoverjie.top/2018/07/04/netty/Netty(2)Thread-model/)

# 机器学习

* [决策树](决策树.md)

# kafka

* [kafka简介](kafka简介.md)
* [kafka运维总结](kafka运维总结.md)


# Http

* [restful学习](restful学习.md)
* [https如何保证加密的安全性](https://www.jianshu.com/p/b894a7e1c779)

# Redis

* [redis-pipeline](redis-pipeline.md)
* [redis-String-vs-Hash](redis-String-vs-Hash.md)
* 缓存穿透问题
  * 在启动的时候放很少的流量进来，权重 + 启动时间
  * 提前建连接池，预热服务

# 数据库

* 分库分表
* SQL 调优
* [美团点评分布式ID生成系统](https://tech.meituan.com/MT_Leaf.html)
* [LruCache在美团DSP系统中的应用演进](https://tech.meituan.com/lrucache_practice_dsp.html)
  * 双向链表 + hash
  * 应用
    * 用来存广告数据 adId
  * 演化1：引入LruCache
  * 演化2：因为折中全量更新而引入时效清退机制
  * 演化3：因为锁的问题进行hash分片
  * 演化4：零拷贝技术
* [MySQL索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)
* [LRU原理](https://zhuanlan.zhihu.com/p/34133067)

# 线上问题定位

* [线上问题总结](线上问题总结.md)

# 基础

* [零拷贝](https://blog.csdn.net/u013096088/article/details/79122671)

* [QPS限流算法](https://blog.csdn.net/tianyaleixiaowu/article/details/74942405)

* [分布式事务](分布式事务.md)

* [闭包](http://www.runoob.com/scala/scala-closures.html)

  * 闭包是一个函数，返回值依赖于声明在函数外部的一个或多个变量。闭包通常来讲可以简单的认为是可以访问一个函数里面局部变量的另外一个函数

  * 如下面这段匿名的函数：`val multiplier = (i:Int) => i * 10  `

  * 这里我们引入一个自由变量 factor，这个变量定义在函数外面。

    这样定义的函数变量 multiplier 成为一个"闭包"，因为它引用到函数外面定义的变量，定义这个函数的过程是将这个自由变量捕获而构成一个封闭的函数。

* 如何实现一个ImmutableSet
