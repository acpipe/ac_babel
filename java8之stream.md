# Java8 之stream操作

# 什么是流
- 不是一种数据结构，更像是一种pipeline的运输数据流。可以理解为Iterator的增强版本。但是区别于Iterator的是java8 lamda可以做并行化处理。
- 产生一个操作结果集合，**但是并不修改源数据**。例如filter操作类似于mysql的视图。

# generate
```java
//collection
collection.parallelStream().forEach(e -> e.toString());
//array
String[] arrays = new String[]{"hello", "world"};
Arrays.stream(arrays).forEach(System.out::println);
Stream.of(arrays).forEach(System.out::println);
//map
key2Value.values().stream().map(List::stream).collect(Collectors.toCollection(LinkedList::new));
...
```

# pipeline

* source.eg : `Collection`, an `array`, a generator `function`.
* intermediate operations. eg : `Stream.filter` or `Stream.map`
* terminal operation.eg : `Stream.forEach` or `Stream.reduce`.

每一个java8的lamda操作都是由这三部分组成的。和spark中的惰性计算类似，中间的`filter`、`map` 这些操作可以被简化。

# 并行化

```java
String[] arrays = new String[]{"hello", "world", "huming", "xiaomi"};
Arrays.stream(arrays).parallel().forEach(System.out::println);
System.out.println("-------------------");
Stream.of(arrays).forEach(System.out::println);
```

```
huming
xiaomi
world
hello
-------------------
hello
world
huming
xiaomi
```

可以显示的调用`paralled` 、`parallelStream`来并行化提高程序执行效率.

# reference

1. [java8文档](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)