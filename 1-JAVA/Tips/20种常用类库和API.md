#### **一、日志相关类库**

日志库是很常见的，因为你在每一个项目中都需要他们。打印日志是服务器端应用中最重要的事情，因为日志是你了解你的程序发生了什么的唯一途径。尽管JDK附带自己的日志库，但是还是有很多更好的选择可用，例如 Log4j 、 SLF4j 和 LogBack。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkTqRPMyZkmm3MCO3l2AXTmZiasBNjfgFyUCk9RW2FZXeZAesrQBcqpZw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Java开发人员应该熟悉日志记录的利弊， 并且了解为什么SLF4J要比Log4J要好。

#### **二、JSON解析库**

在当今世界的web服务和物联网中(IoT)，JSON已经取代了XML，成为从客户端到服务器传送信息的首选协议。有一个好消息和一个坏消息。坏消息 是JDK没有提供JSON库。好消息是有许多优秀的第三方库可以用来解析和创建JSON消息，如 Jackson 和 Gson

![img](https://mmbiz.qpic.cn/mmbiz_jpg/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLks6LBMiaOA6nDPFc2YicCfR2XOeRjwO3VDORiaWGON8DSfCibm2ibDPSXOng/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

一个Java web开发人员应该熟悉Jackson 和 Gson这两种中的至少一种库。

#### **三、单元测试库**

单元测试技术的使用，是区分一个一般的开发者和好的开发者的重要指标。程序员经常有各种借口不写单元测试，但最常见的借口就是缺乏经验和知识。常见的单测框架有 JUnit , Mockito 和PowerMock 。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLk8usRfFAR2QjQSHGq4qnuAvmObib0YoFkUAf1Ng7iadJmsPbkLWD3YApQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **四、通用类库**

有几个很好的第三方通用库可供Java开发人员使用，例如 Apache Commons 和 Google Guava 。我会经常在我的代码中使用这些通用类库，因为这些类库都是经过无数开发者实践过的，无论是实用性还是在性能等方面都是最佳的。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkdibhe6JrjO6LIdp91pRJqqjroHZNTxpmFKB3ObTUfenZp7IAAibbpN7Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **五、Http 库**

我不是很喜欢JDK的一个重要原因就包括他们缺乏对HTTP的支持。虽然可以使用java.net包类，但是这和直接使用像 Apache HttpClient 和 HttpCore 等开源类库比起来麻烦太多了。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkoqBbTuAEvmrFRr7ibraoAcwnufOUbdkVMZiaI4VhOheTMyaJICVb6DicQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

尽管JDK 9将开始HTTP 2.0，也对HTTP的支持做了优化，但是我还是强烈建议所有的Java开发人员熟悉流行的HTTP处理类库，例如HttpClient和HttpCore HTTP等库。

#### **六、XML解析库**

市面上有很多XML解析的类库，如 Xerces , JAXB , JAXP , Dom4j , Xstream 等。 Xerces2是下一代高性能，完全兼容的XML解析工具。Xerces2定义了 Xerces Native Interface (XNI)规范，并提供了一个完整、兼容标准的 XNI 规范实现。该解析器是完全重新设计和实现的，更简单以及模块化。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkE9hR4hauQWSdiaHWgLG3NoMDzRVfkFNn5j04ia2NCsdfAhtLXzPqmykA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **七、Excel读写库**

许多应用程序需要提供把数据导出到Excel的功能，如果你要做相同的Java应用程序,那么你需要 Apache POI API 。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkoHxibxdvs3bEtyPU8gOVjk9EJloRO6wgfuP3sVhx6r4KBKxoZ2BvIDg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这是一个非常丰富的类库，你可以从Java程序读写XLS文件。

#### **八、字节码库**

如果你正在编写一个框架或者类库。有一些受欢迎的字节码库如 javassist 和 Cglib Nodep 可以供你选择，他们可以让你阅读和修改应用程序生成的字节码。

![img](https://mmbiz.qpic.cn/mmbiz_jpg/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkZuPdjKwgRt3Sdlwqp6vWGn2Q2qiatee5Mmm75jJ0KsmORic6jgBPEagA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Javassist使得JAVA字节码操作非常简单。它是一个为编辑Java字节码而生的类库。 ASM 是另一个有用的字节码编辑库。

#### **九、数据库连接池库**

如果你的Java应用程序与数据库交互不是使用数据库连接池库的话，那么你就大错特错了。因为在运行时创建数据库连接非常耗时并且会拖慢你的程序。所以墙裂建议使用，有些好用的连接池可供选择，如 Commons Pool 和 DBCP 。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLk5T07Cm7b5zgCuwT42waSXhSTSNpYq351JGXAib42NJESHzVPrXMGZgg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在web应用程序中，web服务器通常提供了这些功能。但是在java项目中需要把数据库连接池的类库导入到应用中。

#### **十、消息传递库**

像日志和数据库连接池一样，消息传递也是很多实际的Java项目中必备的。Java提供了JMS Java消息服务，但这不是JDK的一部分,你需要单独的引入jms.jar。类似地，如果您准备使用第三方消息传递协议， Tibco RV 是个不错的选择。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkq03WbBQgI8gTtSr4H4icp3tNnhvR8ZiazoxWpSqENKYQdIibZcVmVV0Iw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **十一、PDF处理库**

除了Excel和Word，PDF也是一种常用的文件格式。如果你的应用程序要支持PDF格式的文件处理，你可以使用 iText 和 Apache FOP 类库。两者都提供了非常有用的PDF处理功能。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkyX0udibVuFtBJvevEcen64o9uxZynNqDqXGTLeJUKSx0ECR9Vcn7kcQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **十二、日期和时间库**

在Java之前，JDK的日期和时间库一直被人们所诟病，比如其非线程安全的、不可变的、容易出错等。很多开发人员会选择更好用的 JodaTime 类库。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkcuqTjJicQqYLKl5thj1x0Uia3Kjuy2Vt257nrSuF3lcuu2xITfsyfLrg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

但是在Java8推出之后，我们就可以彻底放弃JodaTime了，因为Java 8提供了其所有功能。但是，如果你的代码运行在一个低版本的JDK中，那么JodaTime还是值得使用的。

#### **十三、集合类库**

虽然JDK有丰富的集合类，但还是有很多第三方类库可以提供更多更好的功能。如 Apache Commons Collections 、 Goldman Sachs collections 、 Google Collections 和 Trove 。Trove尤其有用，因为它提供所有标准Collections 类的更快的版本以及能够直接在原语（primitive）（例如包含int 键或值的Map 等）上操作的Collections 类的功能。

![img](https://mmbiz.qpic.cn/mmbiz_jpg/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkgmsKmPDhQddoyJ2CmCuicBLW93Hw8MQicWiaiaAj6wEyQdmiaAnibDkzKsXQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

FastUtil是另一个类似的API，它继承了Java Collection Framework，提供了数种特定类型的容器，包括映射map、集合set、列表list、优先级队列（prority queue），实现了java.util包的标准接口（还提供了标准类所没有的双向迭代器），还提供了很大的（64位）的array、set、list，以及快速、实用的二进制或文本文件的I/O操作类。

#### **十四、邮件API**

javax.mail 和 Apache Commons Email 提供了发送邮件的api。它们建立在JavaMail API的基础上，提供简化的用法。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkicvoD5kh0jEOWbA0G3Z94W2Heu22AuzDrGMUyCpqo2Dwge654618R7A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **十五、HTML解析库**

和XML与JSON类似，HTML是另外一种我们可能要打交道的传输格式。值得庆幸的是，我们有jsoup可以大大简化Java应用程序使用HTML。你不仅可以使用 JSoup 解析HTML还可以创建HTML文档。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkQEZ17Uia6J4cdt8Jtr6ibabic0H9DfuHfMxvduFuriaJfnziaHUxKjynlFw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **十六、加密库**

Apache Commons家族中的 Commons Codec 就提供了一些公共的编解码实现，比如Base64, Hex, MD5,Phonetic and URLs等等。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLk9uH3VIbciclicepjMER8jItbDjXw9A1a5UpbfRpQ2GbKCNWjJGkVpCDw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **十七、嵌入式SQL数据库库**

我真的是非常喜欢像 H2 这种内存数据库，他可以嵌入到你的Java应用中。在你跑单测的时候如果你需要一个数据库，用来验证你的SQL的话，他是个很好的选择。顺便说一句,H2不是唯一嵌入式DB，你还有 Apache Derby 和 HSQL 可供选择。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkm69cKibmrhov9JRm18UvFc3pqMCNgEppR7icxjGGQ5uBiaZ17jh0vfneg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **十八、JDBC故障诊断库**

有不错的JDBC扩展库的存在使得调试变得很容易，例如P6spy，这是一个针对数据库访问操作的动态监测框架，它使得数据库数据可无缝截取和操纵，而不必对现有应用程序的代码作任何修改。 P6Spy 分发包包括P6Log，它是一个可记录任何 Java 应用程序的所有JDBC事务的应用程序。其配置完成使用时，可以进行数据访问性能的监测。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkxtjpz1ZgrxXF8rmlPT0qext6Yic3zibf0BHcGZor6Xq9oF9Dhk2SOwAg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **十九、序列化库**

Google Protocol Buffer是一种轻便高效的结构化数据存储格式，可以用于结构化数据串行化，或者说序列化。它很适合做数据存储或 RPC 数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化结构数据格式。目前提供了 C++、Java、Python 三种语言的 API。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLk7Dbc3jDyl9HtMVVE8hMKKN3jrIfDojzznGibXF3ibjibJRw6nqjicHWcyQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### **二十、网络库**

一些有用的网络库主要有 Netty 的和 Apache MINA 。如果您正在编写一个应用程序，你需要做的底层网络任务，可以考虑使用这些库。

![img](https://mmbiz.qpic.cn/mmbiz_png/dwoMQBQlZ1QRibiahgyF7SPUJz14zuzaLkqMPxfS3ViandrNDOKGHb6MOibE9NuohWESImeOdBNYx9sia6qhYcsf5dw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)