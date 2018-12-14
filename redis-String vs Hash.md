



# 为什么HSET更节省空间

# String 

```java
typedef char *sds;


struct sdshdr {

    // buf 已占用长度
    int len;

    // buf 剩余可用长度
    int free;

    // 实际保存字符串数据的地方
    char buf[];
};
```

这里除了buf[]之外，还需要额外的存储两个int，这两个int = 4byte 所以，每一个key 需要多出 8 byte作为额外的存储。

## ziplist 

因为 ziplist 节约内存的性质， 哈希键、列表键和有序集合键初始化的底层实现皆采用 ziplist。那具体怎么节约内存呢？

hash中的一个entry :

```
area        |<------------------- entry -------------------->|

            +------------------+----------+--------+---------+
component   | pre_entry_length | encoding | length | content |
            +------------------+----------+--------+---------+
```





# 相关文档

* [HSET vs SET ](https://stackoverflow.com/questions/12779372/hset-vs-set-memory-usage)