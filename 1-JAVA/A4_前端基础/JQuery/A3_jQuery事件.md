### jQuery事件

#### 1.事件绑定

* 传统js 一般一个对象只能绑定某种事件一个函数
* jQuery支持对同一个对象，同一个事件可以绑定多个函数

#### 2.写法

* 绑定事件函数到对象有两种写法

#### 3.事件一次性绑定和自动触发

* 一次性事件one(type,[data],fn)为对象绑定一次性事件，只有一次有效触发事件trigger(type,[data])触发目标对象指定的事件执行

  ```javascript
  $(document).ready(function(){
      $("#d1").one("click",function(){
          $(this).css("background-color","#ADFF2F");
      });
  
      $("#btn1").click(function(){
          //点击按钮1，触发按钮2
          $("#btn2").trigger("click");
      });
  
      $("#btn2").click(function(){
          alert("按钮2的单击事件！")
      });
  });
  ```

#### 4.事件切换

* hover(mouseover,mouseout) 模拟鼠标悬停事件

  ```javascript
  $("#d1").hover(
      function(){
          $(this).css("background-color","#98FB98")
      },function(){
          $(this).css("background","#FFB6C1");
      });
  ```

* toggle(fn1,fn2,fn3...);鼠标点击一次触发一个函数

  ```javascript
  $("#img1").toggle(
      function(){
          $(this).attr("src","../img/dog2.png");
      },function(){
          $(this).attr("src","../img/dog3.png");	
      },function(){
          $(this).attr("src","../img/dog4.png");	
      },function(){
          $(this).attr("src","../img/dog5.png");	
      });
  ```

  

#### 5.事件阻止默认动作和传播

* `event.preventDefault();`	//阻止事件
* `event.stopPropagation();` //阻止事件冒泡

```javascript
$(function(){
    //event 阻止事件的发生
    $("#dellink").click(function(event){
        var isConfrim = confirm("确定删除？");
        if(!isConfrim){
            //阻止事件
            event.preventDefault();
        }
    });
    $("div").click(function(){
        alert($(this).html());
    });

    $("p").click(function(event){
        alert($(this).html());
        event.stopPropagation();
    });
});
```

