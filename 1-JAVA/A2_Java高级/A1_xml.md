### XML

* 可拓展标记性语言（Extensible Markup Language）

1. 入门
2. XML解析
3. XML创建

* 作用：配置、数据交互

* HTML & XML：

  1. 都是基于SGML(标准通用标记语言)分支
  2. HTML程序标记语言(html5不属于SGML)
     * xhtml 严格模式--基于xml后出的
  3. 描述型语言--xml--1998正式形成
  4. 两者都由W3C维护

  * xml：Extensible Markup Language 可扩展性标记语言，xml只代表数据本身，而不包含任何样式，所以也称之为数据描述语言
    * 作用：
      1. 实现不同平台之间的数据交互（webservice : soap协议）
      2. xml还可以用于一些应用程序的配置文件（框架、tomcat、servlet）

* XML文档格式

  1. xml指令<?xml version = "1.0" encoding="utf-8" />
     * 主要描述xml版本（目前为1.0）,编码
  2. 文档类型定义：<!DOCTYPE>标准的
     * 定义是否是一个独立文件，文档类型定义（DTD,XSD）规范文档中允许出现的标记，属性，以及标记之间的关系
  3. 文档元素部分
     * 标签、属性、文本
     * xml元素：由一对标签及标签对之间的内容构成
     * xml标签：主要包含开始标记以及结束标记
     * xml属性：属性通常定义在元素的开始标记中

* XML规范

  1. 标记必须相对HTML来说比较严格
  2. 标签严格区分大小写
  3. 属性值必须使用双引号包含
  4. 属性值中不能包含特殊符号（<,&....）
  5. xml支持类似于html中实体(`&lt;`,`&gt;`...)
  6. 注释代码不能嵌套

* CDATA（未解析的）字符数据

  ```xml
  <![CDATA[	
  	<html>
  		<head></head>
  	</html>
  ]]>
  ```

* AJAX&XML

  * AJAX：异步的JavaScript and xml
  * JSON：是一种轻量级的数据交换格式，实现不同平台之间的数据交互

### XML解析

1. DOM解析

2. SAX解析

3. JDOM解析

4. DOM4j

   Android pull解析

#### DOM解析

* 将需要被解析的文档完整的加载到内存中，解析为一颗倒置的文档树，可以通过解析器任意获取文档树的节点

* 优点：

  * 适合解析较小的文档，解析速度快，可以任意搜索节点
  * 允许应用程序对数据结构做出更改
  * 访问是双向的，可以在任何时候在树中上下导航，获取和操作任意部分的数据

* 缺点：一次性加载整个文档，会消耗大量内存，无法解析过大文档

* 解析详解：

  * 构建Document对象

    ```java
    DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
    DocumentBuilder db = bdf.newDocumentBuilder();
    InputStream is = Thread.currentThread().getContextClassLoader().getResourceAsStream(xml文件);
    ```

  * 遍历DOM对象

    * Document: 	XML文档文件，由解析器获取
    * NodeList:       节点数组 
    * Node:             节点（包括element、#text）
    * Element:        元素，可用于获取属性参数

* 实例代码

  ```xml
  <users>
      <user id="20180901">
          <name>张三</name>
          <sex>男</sex>
          <age>18</age>
          <ismarray>未婚</ismarray>
      </user>
  </users>
  ```

  ```java
  //实例化一个工厂
  DocumentBuilderFactory fatory = DocumentBuilderFactory.newInstance();
  
  //获取解析对象
  DocumentBuilder builder =  fatory.newDocumentBuilder();
  
  //获取文档对象并解析文档
  Document document = builder.parse(new File("src/userInfo.xml"));
  
  //获取第一个user节点中的属性名为id的属性值
  String id = document.getElementsByTagName("user").item(0).getAttributes().getNamedItem("id").getNodeValue();
  //获取user节点的子节点
  String name = document.getElementsByTagName("name").item(0).getTextContent();
  String sex = document.getElementsByTagName("sex").item(0).getTextContent();
  String age = document.getElementsByTagName("age").item(0).getTextContent();
  String ismarray = document.getElementsByTagName("ismarray").item(0).getTextContent();
  
  User user = new User(id, name, sex, age, ismarray);
  
  System.out.println(user);
  ```

#### SAX解析

* Simple API for XML解析

* 基于事件驱动的方式以类似流媒体（一边解析一边使用）的方式进行解析，在读取到一部分内容之后立即开始解析，直到读取到文档结束标记停止

* 优点：适合解析较大文档，解析效率较快，一边读一边解析

* 缺点：无法任意搜索节点，比较难以实现并行搜索

* 原理：简单来说就是对文档进行顺序扫描，当扫描到文档（document）开始与结束、元素（element）开始与结束时通知事件

  处理函数（回调函数），进行相应处理，知道文档结束

* 事件处理器类型

  * 访问XML DTD: DTDHandler
  * 低级访问解析错误：ErrorHandler
  * 访问文档内容：ContextHandler

* DefaultHandler类

  * SAX事件处理程序的默认基类，实现了DTDHandler、ErrorHandler、ContextHandler和EntityResolver接口，通常做法是，继承该基类，重写需要的方法，如startDocument()

* 创建SAX解析器

  ```java
  SAXParserFactory saxf = SAXParserFactory.newInstance();
  SAXParser sax = saxf.newSAXParser();
  ```

  > 关于遍历：
  >
  > ​		①深度优先遍历(`Depthi-First Traserval`)
  > 		②广度优先遍历(`Width-First Traserval`)

* 实例代码

  ```java
  public class Parse03_BySAX  extends DefaultHandler{
      public Book book;
      private String nowTag;
  
      List<Book> books = new ArrayList<Book>();
  
      //文档开始时触发
      public void startDocument() throws SAXException {
          System.out.println("开始解析!");
      }
  
      //读取到开始标记时触发
      /**
  	 *  uri		命名空间的uri地址
  	 *  localName 	不带命名空间前缀的标签名称
  	 *  qName 	包含命名空间的标签名
  	 *  attribute	元素中属性列表
  	 */
      public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
          System.out.println("读取到开始标记:"+qName);
          //记录当前读取到的开始标记
          nowTag = qName;
  
          //当读取到一个开始的book标记后创建一个book对象
          if("book".equals(qName)){
              book = new Book();
              book.setNo(attributes.getValue("no"));
          }
      }
  
      //读取到文本节点时触发
      public void characters(char[] ch, int start, int length) throws SAXException {
          System.out.println("读取到文本节点:"+new String(ch,start,length));
          String value = new String(ch, start, length);
  
          //根据当前记录的标记决定将文本值设置给对象的哪一个属性
          if("name".equals(nowTag)){
              book.setName(value);
          }else if("author".equals(nowTag)){
              book.setAuthor(value);
          }else if("public".equals(nowTag)){
              book.setPublish(value);
          }else if("price".equals(nowTag)){
              book.setPrice(value);
          }
          //将节点置为null
          nowTag="";
      }
  
      //读取到结束标记时触发
      public void endElement(String uri, String localName, String qName) throws SAXException {
          System.out.println("读取到结束标记:"+qName);
          //判断是否读取到book结束
          if("book".equals(qName)){ 
              books.add(book);
          }
      }
  
      //文档结束时触发
      public void endDocument() throws SAXException {
          System.out.println("结束解析!");	
      }
  
      public static void main(String[] args) throws ParserConfigurationException, SAXException, IOException {
  
          //创建解析器工厂
          SAXParserFactory factory = SAXParserFactory.newInstance();
          //生产解析器
          SAXParser parse = factory.newSAXParser();
  
          Parse03_BySAX pbs = new Parse03_BySAX();
  
          //开始解析
          parse.parse("src/book.xml", pbs);
  
          List<Book> list = pbs.books;
  
          System.out.println(list);
      }
  }
  ```

#### JDOM

* Java-based Document Object Model

* Java特定的文档对象模型。自身不包含解析器，使用SAX

* 优点：

  * 使用具体类而不是接口，简化了DOM的API
  * 大量使用了Java集合类，方便了Java开发人员

* 缺点：

  * 没有较好的灵活性
  * 性能较差

* 解析详解

  * 指定解析器

    ```java
    SAXBuilder builder =new SAXBuilder(false);//使用默认解析器
    ```

  * 得到Document对象

    ```java
    Document doc = builder.build(xmlpath);
    ```

  * 得到根节点

    ```java
    Element rootElement = doc.getRootElement();
    ```

  * 得到节点元素集合

    ```java
    List list = rootElement.getChildren(tag);
    
    ```

* 实例代码

  ```java
  public static void main(String[] args) throws JDOMException, IOException {
  
      //创建解析器
      SAXBuilder builder = new SAXBuilder();
      //解析指定文档为一个Document对象
      Document document =  builder.build(new File("src/book.xml"));
      //获取文档的根节点
      Element root = document.getRootElement();
      //获取所有的子节点
      List<Element> list  = root.getChildren("book");
      //遍历所有的book节点
      for(Element e:list){
          //取出节点中指定的属性值
          String no = e.getAttributeValue("no");
          //获取当前节点下指定名称的单个子节点，并取到节点中的文本值
          String name = e.getChild("name").getText();
          String author= e.getChild("author").getText();
          String publish = e.getChild("publish").getText();
          String price = e.getChild("price").getText();
  
          System.out.println( no + name + author + publish +price);
          System.out.println("----------");
      }
  }
  ```

#### DOM4J

* Document Object Model for Java

* 简单易用，采用Java集合框架，并完全支持DOM、SAX和JAXP

* 优点：

  * 大量使用了Java结合类，方便Java开发人员，同时提供一些提高性能的替代方法
  * 支持XPath
  * 有很好的性能

* 缺点：

  * 大量使用接口，API较为复杂

* 解析详解

  * 指定解析器

    ```java
    SAXReader reader = new SAXReader();
    ```

  * 得到Document对象

    ```java
    Document doc = reader.read(xmlpath);
    ```

* 实例代码

  ```java
  public static List<Book> Parse(File file) throws DocumentException{
      List<Book> books = new  ArrayList<Book>();
      Book book = null;
  
      SAXReader reader = new SAXReader();
      Document document = reader.read(file);
      List<Node> list = document.selectNodes("books/book");
  
      for(Node node : list){
          String no = node.valueOf("@no");
          String name = node.selectSingleNode("name").getText();
          String author = node.selectSingleNode("author").getText();
          String publish = node.selectSingleNode("publish").getText();
          String price = node.selectSingleNode("price").getText();
  
          book = new Book(no, name, author, publish, price);
          books.add(book);
      }
      return books;
  }
  ```

  