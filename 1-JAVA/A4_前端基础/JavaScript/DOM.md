### HTML DOM

- HTML DOM 定义了访问和操作HTML文档对的标准方法。

- HTML DOM 把 HTML文档呈现为带有元素、属性和文本的树结构（节点数）。

- 获取当前页面的DOM对象

  ```javascript
  console.info(document);
  ```

- 获取文档中指定元素的方式 

  1. 根据id获取元素

     ```javascript
     document.getElementById('元素id');
     
     var divs = document.getElementById('box-id');
     console.info(divs);	
     console.info(divs.textContent);	//获取文本内容
     console.info(divs.className);   //获取class值
     ```

  2. 根据元素的name属性获取元素（一般用于表单元素）

     ```javascript
     document.getElementByName('元素name值')
     
     function checkAll(flag,f){
         //根据元素的name值，获取所有元素
         var cks = document.getElementsByName('Lang');
         for(var i=0; i<cks.length; i++){
             //					cks[i].checked = flag; //全选
             switch(f){
                 case 0:
                     //全选
                     cks[i].checked = flag; 		
                     break;
                 case 1:
                     //反选
                     cks[i].checked = (cks[i].checked ==true)?false:true;
                     break;
                 case 2:
                     //全不选
                     cks[i].checked = false;
             }
         }
     }
     ```

  3. 根据元素的标签获取元素

     ```javascript
     document.getElementsByTagName('元素的标签名称')；
     for(var i = 0;i < lis.length; i++){
         lis[i].style.width = '150px';
         lis[i].style.height = '30px';
         lis[i].style.padding = '5px 10px';
         lis[i].style.marginTop = '1px';
         lis[i].style.backgroundColor = 'rgb(51,51,51)';
         lis[i].style.textAlign = 'center';
         lis[i].style.lineHeight = '30px';
         lis[i].style.color = '#fff';
     }
     ```

  4. 根据元素的class属性获取元素（IE9以下不支持）

     ```javascript
     document.getElemensByClassName('元素的属性值');
     var colors = ['red','green','blue'];
     //根据类名称(divs)获取所有元素
     var divs = document.getElementsByClassName('divs');
     for(var i = 0; i < divs.length; i++){
         divs[i].style.backgroundColor = colors[i];
     }
     ```

  5. 根据CSS的选择器获取单个元素

     ```javascript
     document.querySelector();
     
     var h = document.querySelector('#title');
     console.info(h);
     ```

  6. 根据CSS的选择器获取多个元素

     ```javascript
     document.querySelectorAll();
     
     var hs = document.querySelectorAll(/*'p'*/'.msg'); //数组
     console.info(hs);
     ```

