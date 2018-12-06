# jmap

```shell
jmap -histo [pid]
```

用[工具](https://www.eclipse.org/mat/downloads.php)查看

## 内存dump 分析工具-Eclipse Memory Analyzer Tool

`jmap -dump:live,format=b,file=[文件名] [pid]`

* MAT是什么？个基于Eclipse的内存分析工具，是一个快速、功能丰富的[Java ](http://lib.csdn.net/base/java)heap分析工具，它可以帮助我们查找内存泄漏和减少内存消耗。使用内存分析工具从众多的对象中进行分析，快速的计算出在内存中对象的占用大小，看看是谁阻止了垃圾收集器的回收工作，并可以通过报表直观的查看到可能造成这种结果的对象。

* [下载地址]( http://www.eclipse.org/mat/downloads.php)
* 一般的内存dump文件较大，可以通过调整下载下来的文件夹中 MemoryAnalyzer.ini 来调整MAT使用内存大小， 参数为 -vmargs -Xmx3072m(解决imprt error的问题)

### Histogram

* Class Name ： 类名称，java类名

* Objects ： 类的对象的数量，这个对象被创建了多少个

* Shallow Heap ：一个对象内存的消耗大小，不包含对其他对象的引用

* Retained Heap ：是shallow Heap的总和，也就是该对象被GC之后所能回收到内存的总和

* class name 支持 Regex搜

