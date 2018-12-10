# 原理

一致性hash 算法的原理很简单，见[博客](https://blog.csdn.net/cywosp/article/details/23397179)。

# 实践

一致性hash在rpc中有应用。rpc客户端存储的数据格式如下：

```shell
ip1 + vitual Index 1 -> hash值0
    ...
ip1 + vitual Index 9 -> hash值9
ip2 + vitual Index 1 -> hash值10
    ...
ip2 + vitual Index 9 -> hash值19
    ...
```

现在下游上线或者下线的时候，在zk上的注册都会频繁的改变，那客户端就会重新计算hash，而一旦hash的函数比较复杂的时候，重新计算hash值就会比较耗时。

比如：假如线上有100台机子，有10个虚拟node。那么每一次如果上线的时候都要重新计算一致性hash的值，那么就需要100 * 10次的hash计算。

改进也很简单，部署新的机子才会需要计算hash值。

这样也可能有问题，比如在docker平台上面，每起一个实例的ip都是不一样的，也是会出问题的，所以docker 要尽量保证Ip不变。