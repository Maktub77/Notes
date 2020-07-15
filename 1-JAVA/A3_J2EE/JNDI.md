#### JNDI

* Java Naming and Directory Interface，Java命名和目录接口
* JNDI提供统一的客户端API，通过不同的访问提供者接口JNDI服务供应接口（SPI）的实现，由管理者将JNDI API映射为特定的命名服务和目录系统，使得Java应用程序可以和这些命名服务和目录服务之间进行交互。
* 目录服务是命名服务的一种自然扩展。两者之间的关键差别是目录服务中对象不但可以有名称还可以有属性，而命名服务中对象没有属性
* 集群JNDI实现了高可靠性JNDI，通过服务器的集群，保证了JNDI的负载均衡和错误恢复。在全局共享的方式下，集群中的一个应用服务器保证本地JDNI树的独立性，并拥有全局的JNDI树。每个应用服务器在把部署的服务对象绑定到自己本地的JNDI树的同时，还绑定到一个共享的全局JNDI树，实现全局JNDI和自身JNDI的联系

#### 优点

* 包含了大量的命名和目录服务，使用通用接口来访问不同种类的服务；
* 可以同时连接多个命名或目录服务上
* 建立起逻辑关系，允许把名称同Java对象或资源关联起来，而不必知道对象或资源的物理ID

#### 架构

* JNDI架构提供了一组标准的独立于命名系统的[API](https://baike.baidu.com/item/API)，这些API构建在与命名系统有关的驱动之上。这一层有助于将应用与实际数据源分离，因此不管应用访问的是[LDAP](https://baike.baidu.com/item/LDAP)、[RMI](https://baike.baidu.com/item/RMI)、[DNS](https://baike.baidu.com/item/DNS)、还是其他的[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)。换句话说，JNDI独立于[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)的具体实现，只要有目录的服务提供接口（或驱动），就可以使用目录。

  关于JNDI要注意的重要一点是，它提供了应用[编程接口](https://baike.baidu.com/item/%E7%BC%96%E7%A8%8B%E6%8E%A5%E5%8F%A3)(application programming interface，API)和服务提供者接口(service provider interface，SPI)。这一点的真正含义是，要让应用与命名服务或[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)交互，必须有这个服务的JNDI服务提供者，这正是JNDI SPI发挥作用的地方。服务提供者基本上是一组类，这些类为各种具体的命名和[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)实现了JNDI接口—很像JDBC驱动为各种具体的[数据库系统实现](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B3%BB%E7%BB%9F%E5%AE%9E%E7%8E%B0)了JDBC接口一样。作为一个应用开发者，我们不必操心JNDI SPI的具体实现。只需要确认要使用的每一个命名或[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)都有服务提供者。

#### 组件

1. javax.naming：命名操作
   * 包含了访问命名服务的类和接口。例如，它定义了Context接口，这是命名服务执行查询的入口。 
2. javax.naming.directory：目录操作
   * 对命名包的扩充，提供了访问[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)的类和接口。例如，它为属性增加了新的类，提供了表示目录上下文的DirContext接口，定义了检查和更新目录[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)的属性的方法 。
3. javax.naming.event：在命名目录服务器中请事件通知
   * 提供了对访问命名和[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)时的事件通知的支持。例如，定义了NamingEvent类，这个类用来表示命名/[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)产生的事件，定义了侦听NamingEvents的NamingListener接口 
4. javax.naming.ldap：提供LDAP支持
   * 这个包提供了对LDAP 版本3扩充的操作和控制的支持，通用包[javax.naming.directory](https://baike.baidu.com/item/javax.naming.directory)没有包含这些操作和控制。 
5. javax.naming.spi：允许动态插入不同实现
   * 这个包提供了一个方法，通过[javax.naming](https://baike.baidu.com/item/javax.naming)和有关包动态增加对访问命名和[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)的支持。这个包是为有兴趣创建服务提供者的开发者提供的。 

#### 用途

* 命名或[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)使用户可以[集中存储](https://baike.baidu.com/item/%E9%9B%86%E4%B8%AD%E5%AD%98%E5%82%A8)共有信息，这一点在网络应用中是重要的，因为这使得这样的应用更协调、更容易管理。例如，可以将打印机设置存储在[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)中，以便被与打印机有关的应用使用。

  ​	我们大家每天都不知不觉地使用了命名服务。命名系统中的[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)可以是[DNS](https://baike.baidu.com/item/DNS)记录中的名称、[应用服务器](https://baike.baidu.com/item/%E5%BA%94%E7%94%A8%E6%9C%8D%E5%8A%A1%E5%99%A8)中的EJB组件(Enterprise JavaBeans Component)、[LDAP](https://baike.baidu.com/item/LDAP)(Lightweight Directory Access Protocol)中的用户Profile。

  ​	[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)是命名服务的自然扩展。两者之间的关键差别是[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)中[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)可以有属性（例如，用户有email地址），而命名服务中对象没有属性。因此，在[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)中，你可以根据属性搜索[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)。JNDI允许你访问文件系统中的文件，定位远程RMI注册的[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)，访问像LDAP这样的[目录服务](https://baike.baidu.com/item/%E7%9B%AE%E5%BD%95%E6%9C%8D%E5%8A%A1)，定位网络上的EJB组件。

  ​	对于象LDAP[客户端](https://baike.baidu.com/item/%E5%AE%A2%E6%88%B7%E7%AB%AF)、应用[launcher](https://baike.baidu.com/item/launcher)、类浏览器、网络管理实用程序，甚至地址薄这样的应用来说，JNDI是一个很好的选择。

#### 常用操作

* `void bind(String sName,Object object);`――绑定：把名称同[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)关联的过程
* `void rebind(String sName,Object object);`――重新绑定：用来把[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)同一个已经存在的名称重新绑定
* `void unbind(String sName);`――释放：用来把[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)从目录中释放出来
* `Object lookup(String sName);`――查找：返回目录中的一个[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)
* `void rename(String sOldName,String sNewName);`――[重命名](https://baike.baidu.com/item/%E9%87%8D%E5%91%BD%E5%90%8D)：用来修改[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)名称绑定的名称
* `NamingEnumeration listBinding(String sName);`――清单：返回绑定在特定上下文中[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)的清单列表
* `NamingEnumeration list(String sName);`

代码示例：重新得到了名称、类名和绑定[对象](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1)。 

```java
NamingEnumeration namEnumList = ctxt.listBinding("cntxtName");
...
    while (namEnumList.hasMore()){
        Binding bnd = (Binding) namEnumList.next();
        String sObjName = bnd.getName();
        String sClassName = bnd.getClassName();
        SomeObject objLocal = (SomeObject) bnd.getObject();
    }
```

