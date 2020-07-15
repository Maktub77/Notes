### JS

* JavaScript，是一门基于html的脚本语言（无法独立运行，一般嵌入到html中运行，自从有了NodesJS之后，js可以基于node环境运行），js是基于对象的语言（不是面向对象，没有类的概念，但是有构造器），是一门基于事件驱动（可以通过页面中的课交互控件触发js代码）的语言。由三大核心部分组成：

  1. ECMAScript核心语法（js的基本语法）
  2. DOM模型（Document Object Mode）
  3. BOM模型（Browse Object Mode）

* Java

  * 面向对象，任何对象都需要类来创建
  * 先编译，再解释（运行）
  * 强类型语言（任何数据在使用前必须先声明数据类型；并且数据类型一旦定义，无法修改）
  * 四类八种数据类型

* JS

  * 基于对象（有一系列内置对象，同时也可以创建）
  * 无须编译，可以直接运行
  * 若类型语言（所有数据不必提前声明数据类型，并且在使用时能动态改变数据类型）
  * 大方向：var一种
    * 数字类型只有double（没有整型和小数型区分）
    * 没有字符型，所有字符字符串都是字符串类型
    * JS的数据类型：数字、字符串、布尔类型、null、undefined（未定义的或找不到的）、NaN(Not a Number)、对象类型

* JS在和html中使用的两种方式：

  1. 在html页面的头部，或结束位置（</body>前），使用<script>标签包裹js代码
  2. 写到外部JS文件中，然后引入。
     * 引入使用代码：<script type="text/javascript" src="js的外部文件"></script>

* 强制模式："use strict"

  ```js
  //0、强制模式
  "use strict"
  
  //1、
  var i = 10;
  console.log();
  alert();
  document.write();	/*写整个页*/
  对象.innerHTML="";	/*写局部页面*/
  
  //2、对象
  var j = {}; /*大括号表示对象的意思*/
  console.info(j)
  
  //3、数组
  var k = [];
  console.info(k);
  ```

* 流程控制

  1. 条件语句

     1. if...else

     2. switch

        >  switch在比较时使用"==="

  2. 循环语句

     1. for
     2. while
     3. do...while
     4. for...in

  ```js
  //乘法口诀表——隔行换色
  var color;
  for(var i = 1; i<=9; i++){
      for (var j = 1; j<=i; j++) {
          if(i%2==0){
              color='blue';
          }else{
              color='red';
          }
          document.write('<span style="color:'+color+'">'
                         +j+'*'+ i +'='+(i*j) + 
                         '</span>&nbsp;&nbsp;&nbsp;');
      }
      document.write('<br />');	//在网页中换行
  }
  ```

* JS弹出框

  1. 警告框：alert();
  2. 确认框：confirm();
  3. 信息框：prompt();

* 函数

  * JS是一个事件（鼠标键盘操作...）驱动的程序，就是触发一个函数

  * Function 函数名(参数,变元){

    ​	函数体;

    ​	return...;

    }

    1. 定义函数：

        * 无参无返回函数

        * 无参有返回函数

        * 有参无返回函数

        * 有参有返回函数

           ```js
           function print(){
               console.info('hello');
           }
           ```

    2. 调用函数

       ```js
       //函数调用方法一：
       print(); 
       //函数调用方法二：创建一个作用域 多库多共存
       (function print2() {
           console.info('hello')
       })();
       ```

    3. 匿名函数

       ```js
       var a = function (){
           console.info('匿名函数');
       }
       a();
       ```

    4. 含参有返回函数

       ```js
       function fun1(a,b){
           return a+b;
       }
       var m = fun1(10,5);
       console.info(m);
       ```

       >JS中不存在重写和重载，函数名相同的话就近原则

    5. arguments.length：函数的参数个数

       arguments.callee()：表示自己调用自己

  * 事件触发

    * onblur ：元素失去焦点

    * onfocus ： 元素获取焦点

    * onchange ：框内容改变

    * onclick：鼠标点击一个对象

    * ondblclick：双击

    * onkeydown：按下键盘按键时

    * onkeypress

    * onkeyup

    * onload

      ```js
      <input type="text" name="username" onblur="f_go()" onfocus="f_get()"/><br />
          
      function f_go(){
          alert("焦点移开");
      }
      function f_get(){
          alert("获取焦距");
      }
      ```

* 书写对象

  ```js
  function Myclass(n){
      this.a = n;
      this.b = 1;
      this.c = "abc";
      this.m = function (){
          alert();
      }
  }
  ```

* 数组

  ```js
  //1.new Array(); 数组的定义和赋值
  var arr = new Array();
  arr[0] = "aaa";
  arr[1] = "bbb";
  console.info(arr[1]);
  
  //2.new Array(参数【n】);
  var arr2 = new Array(3,3,4,4,5,6);
  console.info(arr2[5]);
  
  //3.
  var arr3 = ["aaa","bbb","ccc"];
  console.info(arr3[1]);
  console.info(arr3.length);
  
  //4.
  var arr4 = {"1":"zs","a":"ls","2":"ww"};
  console.info(arr4['a']);
  
  //for_in
  for(var k in arr4){
      console.info(k);
      console.info(arr4[k]);
  }
  
  arr5 = arr5.join(); /*将数组转换为字符串*/
  console.info(arr5);
  arr5 = arr5.split(','); /*将字符串按照分隔符分割为数组*/
  
  arr5.push(10); /*在数组的末尾追加元素*/
  console.info(arr5)
  
  var rm=arr5.pop(); /*将数组末尾的元素移除*/
  console.info("移除的元素为：" + rm);
  console.info(arr5);
  ```

* 严格的函数(自定义异常)（调用函数时，要求传入的参数必须与参数个数一致，否则抛出异常）

  ```js
  function fun4(a,b,c){
      try{
          if(arguments.length!=3){
              throw new Error('传入的参数个数为：'+arguments.length+',应该传入3个！');
          }else{
              return a+b+c;
          }
      }catch(e){
          console.info(e);
      }
  }
  
  result = fun4(1,2);
  console.info(result);
  ```

* 控制台显示

  ```js
  <script type="text/javascript">
   //	window.alert('HelloWorld');
      console.info('HelloWorld');
      console.info('nihao');
      console.log('日志');
      console.warn('警告信息');
      console.error('错误信息');
  </script>
  ```

* 获取数据类型typeof()、局部变量、运算符

  ```js
  console.info(typeof(b));	//获取数据的数据类型
  console.info(typeof('hello'));
  
  /*局部变量*/
  var k=100;
  
  /*运算符*/
  var a = '100';
  console.info(k==a); /*==:只判断值；===:判断值和数据类型*/
  
  var age = 18;
  console.info(age>=18?'欢迎光临':'禁止如内');
  ```

* 弹出框

  * alert('确认删除');

  * confirm('确认删除？');

    ```js
    if(confirm('你确定？')){
        alert('你刚刚点击的是--确定');
    }else{
        alert('你刚刚点击的是--取消');
    }
    ```

  * prompt("请输入你的密码","1234");

* 回调

  ```js
  function callme(a){
      console.info('这是一个回调函数'+ a);
  }
  
  function waitme(call,p){
      console.info('等等我');
      call(p);	/*回调函数*/
  }
  var a = callme;
  waitme(a,'hello')
  
  //根据id获取页面中的元素
  var button = document.getElementById('btn');
  
  //当元素对象设置点击事件，触发一个指定名称的函数
  button.onclick = hand();
  
  function hand(){
      console.info('按钮被点击');
  }
  ```

* JS的数据对象

  * JSON对象 JavaScript Object Notation
  * 轻量级数据交换格式，在不同平台之间交互

  ```js
  var p = {
      name:'张三',
      sex:'男',
      age:18,
      heigh:1.7,
      setName:function(name){
          this.name = name;
      },
      getName:function(){
          return this.name;
      }
  };
  //console.info(p.name);
  p.setName('李四');
  console.info(p.getName());
  console.info(p);
  
  //2.创建JSON对象方法
  var user = new Object(); //User user = new Object();
  user.username = 'admin';
  user.password = '123456';
  console.info(user);
  
  //3.创建JSON对象的方法
  function Emp(ename,job,sal,hello){
      this.name = ename;
      this.job = job;
      this.sal = sal;
      this.sayHello = hello;
  }
  
  var emp = new Emp('jack','项目经理','18888',function(){
      console.info('hello,'+this.name);
  });
  console.info(emp);
  emp.sayHello();
  ```

* 闭包（Closure）:是JavaScript一种高级概念，实现不同作用域之间的变量共享

  ```js
  function a(){
      this.s = 10;
      this.increment=function(){
          this.s++;
      }
      this.getS=function(){
          return this.s;
      }
  }
  a();
  var a = new a();
  a.increment();
  a.increment();
  console.info(a.getS());
  
  function fun1(){
      var v = 1;
      var f = function(){
          return v;
      }
      return f;
  }
  var fun = fun1();
  console.info(fun());
  ```

  
