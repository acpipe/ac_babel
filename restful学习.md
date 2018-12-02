关于restful的文章，网上文章搞得我云里雾里，花了点时间整理了一下。

# rest, restful,restful-api

rest, restful, restful, restful-api这几个词经常在各种博客里面出现.具体什么关系呢?这个要从软件架构设计风格说起.

[**SOA(Service-oriented architecture)**](https://en.wikipedia.org/wiki/Service-oriented_architecture): 软件的主要元素是服务.

[**ROA(Resource-oriented architecture)**](https://en.wikipedia.org/wiki/Resource-oriented_architecture): 软件的主要元素是资源.

这其实有点像面向过程编程和面向对象编程的区别。SOA视角中,软件就是由各种松耦合的服务组成的,一切都可以服务化.而在ROA视角中,软件是由资源组成的,一切软件都可以通过对资源的操作(CRUD)来完成.

**Rest(Resource Representational State Transfer )**:ROA的实现有很多种.具体怎么来实现以资源为中心的软件架构? rest就是ROA在网络软件应用中的一种定义, 而restful服务是一种rest架构风格下的实现,引用一下wikipedia上的一段话。


> REST, describes a series of architectural constraints that exemplify how the web's design emerged (ROA).

> Representational State Transfer (REST) is an architectural style that defines a set of constraints and properties based on HTTP. Web Services that conform to the REST architectural style, or RESTful web services, provide interoperability between computer systems on the Internet. 

好像还是不太理解对**资源**的操作和**服务化**的区别。举个例子就好了：

就我当前维护的广告系统来说,假如我要对一个广告主的账户余额进行更改。

基于服务我们会这么考虑问题：
1. 账户相关的模块服务化,提供一个接口出来.
2. 然后开始写**相关逻辑**.
3. 写相关逻辑的时候需要check这个账户是不是合法账户，需要登录验证等操作
4. 这些内部操作最后对调用方透明的，只需要给出一个updateBilling的接口.

基于资源我们会这么考虑问题：
1. 对账户是不是合法我们可以用GET来对**账户资源**实现.
2. 验证操作我们可以用GET来对**账户信息资源**实现.
3. 更新账户余额我们可以用POST来对**账户余额资源**实现.

所以，我开头才会说这有点像面向对象和面向过程的区别.

## **restful约束/优点**

前面说restful系统遵循一定的约束，具体有哪些约束呢？满足如下6条的约束就可以被称作rest风格的架构，下面摘自维基百科。

1.客户端-服务器结构。通过一个统一的接口来分开客户端和服务器，使得两者可以独立开发和演化。客户端的实现可以简化，而服务器可以更容易的满足可伸缩性的要求。

2.无状态。在不同的客户端请求之间，服务器并不保存客户端相关的上下文状态信息。任何客户端发出的每个请求都包含了服务器处理该请求所需的全部信息。

3.可缓存。客户端可以缓存服务器返回的响应结果。服务器可以定义响应结果的缓存设置。

4.分层的系统。在分层的系统中，可能有中间服务器来处理安全策略和缓存等相关问题，以提高系统的可伸缩性。客户端并不需要了解中间的这些层次的细节。

5.按需代码（可选）。服务器可以通过传输可执行代码的方式来扩展或自定义客户端的行为。这是一个可选的约束。

6.统一接口。该约束是 REST 服务的基础，是客户端和服务器之间的桥梁。该约束又包含下面 4 个子约束。

- 资源标识符。每个资源都有各自的标识符。客户端在请求时需要指定该标识符。在 REST 服务中，该标识符通常是 URI。
- 通过资源的表达来操纵资源。客户端根据所得到的资源的表达中包含的信息来了解如何操纵资源，比如对资源进行修改或删除。
- 自描述的消息。每条消息都包含足够的信息来描述如何处理该消息。
- 超媒体作为应用状态的引擎（HATEOAS）。客户端通过服务器提供的超媒体内容中动态提供的动作来进行状态转换。

解释一下啥是[HATEOAS](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/HATEOAS)（**Hypermedia As The Engine Of Application State)**。客户端与服务器的交互完全由超媒体动态提供，客户端无需事先了解如何与数据或者服务器交互，所有的客户端后续行为完全由服务端的返回决定。相反，如果是SOA风格需要定义IDL。举例:

如下获取账户资源的请求，需要返回用XML格式表示。

```
GET /account/12345 HTTP/1.1
Host: bank.example.com
Accept: application/xml
...
```

返回结果如下：

```
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: ...

<?xml version="1.0"?>
<account>
    <account_number>12345</account_number>
    <balance currency="usd">100.00</balance>
    <link rel="deposit" href="https://bank.example.com/accounts/12345?deposit=12345" />
    <link rel="withdraw" href="https://bank.example.com/accounts/12345?withdraw=12345" /> 
    <link rel="transfer" href="https://bank.example.com/accounts/12345?transfer=2345" />
    <link rel="close" href="https://bank.example.com/accounts/12345?status=close" />
</account>
```

表示现在可以执行：存款、提款、转账、销户的操作，这些操作放在了link中。

假如账户余额不足，返回结果会是：

```
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: ...

<?xml version="1.0"?>
<account>
    <account_number>12345</account_number>
    <balance currency="usd">-25.00</balance>
    <link rel="deposit" href="https://bank.example.com/account/12345?deposit=12345" />
</account>
```

这样link中的结果可执行的操作只有存款这一种, 其他的链接是不可用的，所以称之为：**应用状态引擎（Engine Of Application State）。**客户端可能的状态依赖于资源的状态，客户端无需知道每种描述信息和交互机制，如果需要解析新的信息格式，可以依赖于第五点：按需定制代码。

最大的特点是落到**基于资源、接口统一**上。把所有东西抽象成资源，把**操作协议统一**了，系统之间的配合就有了一定的约束，将减小服务对接(撕逼)成本，这是Self-descriptive messages的基础， cache的基础。至于分层啥的，我觉得这不是每种架构风格的标配么。

## **REST vs SOA**

REST-service

- 网络上的资源都被抽象为资源，这些资源都具有唯一的统一资源标识符(URI：Uniform Resource Identiter)
- 通过HTTP协议的标准动作(Get、Put、Post、Delete)通过统一的接口对资源进行操作
- 无状态
- 基于HTTP
- 可以描述信息，比如信息是XML、zip、json、pdf

SOA-service

- 通过网络终结点对外提供服务。
- 通过调用服务接口实现软件
- 可以有状态
- 可以基于多种协议
- 信息需要依赖于接口对接双方约定.

## **restful-api设计**

这里推荐一下:[阮一峰的博客](https://link.zhihu.com/?target=http%3A//www.ruanyifeng.com/blog/2014/05/restful_api.html)，毕竟我没有在大型复杂项目中实践过。不过文中说把版本号放url中，另一篇博客[理解RESTful架构](https://link.zhihu.com/?target=http%3A//www.ruanyifeng.com/blog/2011/09/restful)又说最好版本号不要放在url中，应该是错误吧，知道的同学回答下。

## 相关资料

- [REST会是SOA的未来吗？](https://link.zhihu.com/?target=http%3A//www.infoq.com/cn/articles/RESTSOAFuture/%23theCommentsSection)
- [理解RESTful架构](https://link.zhihu.com/?target=http%3A//www.ruanyifeng.com/blog/2011/09/restful.html)
- [Representational state transfer](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Representational_state_transfer)
- [REST风格的优势是什么？](https://www.zhihu.com/question/33959971)
- [https://www.ibm.com/developerworks/cn/java/j-lo-SpringHATEOAS/](https://link.zhihu.com/?target=https%3A//www.ibm.com/developerworks/cn/java/j-lo-SpringHATEOAS/)

* (https://www.zhihu.com/question/33959971)
