* viewport:页面支持移动端

  ```html
  <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
  ```

* display:flex;  弹性盒子

  ```html
  .footer-container{
      display: flex; /*弹性盒子*/
  }
  footer>span{
      flex: 1;
  }
  ```

* overflow:hidden;  超出容器范围元素：隐藏

* 浮动：主要用来做自适应（随着页面的大小变化而变化）

  * 清除浮动：clear:both;

* CSS优先级：

  * !important>id>class>元素选择器>兄弟选择器...

* 过渡:transition 

  ```css
  .btn-info{
      background-color: rgb(1,216,103);
      transition: background-color 2s 3s; /*颜色3秒后开始过渡2s*/
      transition: all 0.5s; /*所有样式过渡0.5s*/ 
      transition: all 1s ease-out;
  }
  ```

* 动画:transform

  ```css
  img:hover{
      /*transform: scale(2.0);*/	/*放大倍数*/
      /*transform: rotate(180deg);*/	/*旋转*/
      /*transform: translateX(200px);*/	/*平移*/
      /*infinita:不停的  linear:均匀的*/
      animation: rotate .01s infinite linear;
      border-radius: 50%;	
  }
  /*自定义动画*/
  @keyframes rotate{
      0%{
          transform: rotate(0deg);
      }
      100%{
          transform: rotate(360deg);
      }
  }
  ```

* 聚焦效果：focus

  ```css
  .input:focus{ 	/*聚焦效果*/
      border-color: rgb(58,179,255);
      box-shadow:0 0 10px rgba(58,179,255,.5)  ;
  }
  ```

* 透明度：opacity