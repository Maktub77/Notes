### AJAX

* 异步 JavaScript and xml : 提供一个与后台交互的重要组件

* XMLHttpRequest :通过一种通用的数据交换格式就可以实现前后端数据的交互（JSON ）

* 可以通过AJAX实现异步刷新技术

  > JS中任何数据类型都可以转换为boolean类型
  > ​	null /undifiend /NaN 布尔类型中表示为false

* AJAX的使用步骤：

  1. 判断浏览器是否支持，并获取XMLHttpRequest
  2. 打开到指定的服务端连接
  3. 发送请求
  4. 准备一个回调函数处理服务端响应的结果

  ```JS
  var xhr;
  function getXHR(){
      //1.判断浏览器是否支持XMLHttpRequest
      if(window.XMLHttpRequest){
          xhr = new XMLHttpRequest();
      }else{
          xhr = new ActiveXObject('micrsoft.XMLHTTP');
      }
  }
  
  //登录
  function login(){
      getXHR();
      //打开链接
      //参数1:请求发送，参数2:服务器医院地址 参数3:是否异步提交
      xhr.open('get','json/data.json',true);
  
      //3.发送请求
      xhr.send();
  
      //4.当请求状态发生改变时执行回调函数
      xhr.onreadystatechange = callback;
  }
  
  function callback(){
      //获取请求状态
      if(xhr.readyState == 4){
          //获取服务端的响应状态
          if(xhr.status == 200){
              //获取服务端响应回来的数据
              var data = xhr.responseText;
              //将json字符串转换为json对象
              data = JSON.parse(data); 
              console.info(data);
          }
      }
  }
  ```


### jQuery的Ajax编程

- 回顾传统的Ajax开发步骤

  ```javascript
  //1.判断浏览器是否支持XMLHttpRequest
  //创建XMLHttpRequest对象
  function getXHR(){
      if(window.XMLHttpRequest){
          xhr = new XMLHttpRequest; 
      }else{
          xhr = new ActiveXObject('Micrsoft.XMLHTTP');
      }
  }
  //2.绑定回调函数
  xhr.onreadystatechange = function(){
      ... ...
  }
  //3.建立连接
      xhr.open("get","url",true); //是否异步提交
  //4.发送数据
      xhr.send(null) //get请求
  //5.书写回调函数
      if(readState == 4){
          if(status == 200){
              ... ...
              //操作xhr.responseText主要针对返回HTML片段和json
              //操作xhr.responseXML主要针对返回XML片段
          }
      }
  ```

- 为了简化Ajax开发， jQuery提供了对`$.ajax()` 进一步的封装方法 `$load、 $get、 $post`。这三个方法不支持跨域 

- `$getJSON`、 `$getScript`支持跨域。

```javascript
  $.ajax({
      type:"get",
      url:"data.jso",
      dataType:"json",
      success:function(data){
          alert("请求成功："+data.people[0].firstName);
      },
      error:function(data){
          alert("请求失败："+data.status);
      }
  });
```

- load方法

  ​	load方法是jQuery中最为简单和常用的Ajax方法， 处理HTML片段此方法最为合适。
  ​	语法:
  ​	`$("jquery对象").load("url","data") ;`

  - url： Ajax访问服务器地址
  - data： 请求参数
  - 返回内容HTML片段 ， 自动放入`$("jquery对象")innerHTML `中(如果返回的数据需要处理， 我们可以使用get或者post)

  ​	load()方法的传递参数根据参数data来自动自定。 如过没有参数的传递， 采用GET方式传递， 否则采用POST方式	

  ```javascript
  $("#show").load("data.json");	
  ```

- get方法和post方法

  `$.get/$.post("url","parameter",function(data){...});`

  - url       Ajax访问服务器地址
  - parameter  代表请求参数
  - function  回调函数 data 代表从服务器返回数据内容
  - 这里data代表各种数据内容 ： HTML片段、 JSON、 XML

- 如果传递参数给服务器使用 `$.post` , 不需要传参数 可以使用 `$.get`

```javascript
$.get("data.json",function(data){
    alert("请求成功："+data.people[1].firstName);
});

$.post("data.json",function(data){	//post请求响应服务器
    alert("请求成功："+data.people[1].firstName);	
});
```

