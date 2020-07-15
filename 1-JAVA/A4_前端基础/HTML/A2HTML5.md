## HTML5

1. HTML5的介绍

   * 超文本标记性语言，一个网页的基本框架结构（骨架）

2. HTML-格式标签

   ```html
   <p>
       <font face="仿宋" color="#FF0000" size="7">
           <center>
               七 夕 快 乐
           </center>
       </font>
   </p>    
   ```

   * 特殊符号:空格`&nbsp`  & `&amp`  `&quot`  `&reg` `&copy` `&traed`  >`&gt`  < `&lt`  上标`&sup` 下标`&sub`	

3. HTML-文本标签

   ```html
   <p>正常文本</p>
   <i>斜体文本</i>
   <b>粗体文本</b>
   <u>下划线文本</u>
   <s>删除文本</s>
   显示注释<!-- ctrl+shift+?-->
   hr:分割线标签
   <hr color="red" size="35px" width="200">
   像素：单位面积里点的个数，点越多像素越高
   <h1>一级标签</h1>
   <h3>三级标签</h3>
   <h6>六级标签</h6>
   <em>强调文本</em>
   <address>地址、作者、时间</address>
   <address>环球时报 作者 2018-08-10</address>
   <marquee>这是一个飘动的小广告</marquee>
   ```

4. HTML-超链接标签

   ```html
   <a href="http://www.baodu.com">百度一下，你就知道</a>
   <p>href="../aaa/lesson01"</p>
   <p>返回上一级查找aaa文件夹下lesson01</p>
   
   <!--a[href=#]{这是一个超链接$$}*3 + Tab-->
   <a href="#">这是一个超链接01</a>
   <a href="#">这是一个超链接02</a>
   <a href="#">这是一个超链接03</a>
   ```

5. 列表标签

   * `dl-dt-dd`

     ```html
     <dl>
         <dt>商品管理</dt>
             <dd>商品上架</dd>
             <dd>商品日期</dd>
         <dt>订单管理</dt>
             <dd>订单列表</dd>
             <dd>订单处理</dd>
     </dl>
     ```

   * 无序列表 `ul-li`

     ```html
     <ul>
         <li type="circle">公司概况</li> 
         <li type="square">最新新闻</li>
         <li type="disc">关于我们</li>
     </ul>
     ```

   * 有序列表 `ol-li`

     ```html
     <ol>
         <li>上课不准玩手机</li>
         <li>严禁在教室用餐</li>
     </ol>
     ```

6. HTML-多媒体标签

   ```html
   <h2>音频</h2>
   loop=-1不循环 autoplay:自动播放 controls播放工具 hidden:隐藏
   <audio src="audio/123.mp3" loop="-1" autoplay controls hidden="hidden">
       你的浏览器太low
   </audio>
   
   <h2>视频</h2>
   <!--video播放格式有限-->
   <video width="300" autoplay controls>
       <source src="video/Cayenne.mov"></source>
   	<source src="video/Cayenne.mov"></source>
   	浏览器low
   </video>
   ```

7. HTML-表格/框架标签

   ```html
   table>tr*2>td*3
   border:线条
   cellpadding:列中的内容与边框之间的距离
   cellspacing:单元格与单元格之间距离
   colspan:合并列
   rowspan:合并行
   <table border="1px" cellpadding="0px" cellspacing="0px">
       <tr>
           <td></td>
           <td></td>
           <td></td>
       </tr>
       <tr>
           <td></td>
           <td></td>
           <td></td>
       </tr>
   </table>
   ```

8. HTML-表单标签

   ```html
   <!--action:form表单提交的地址-->
   <!--method:form表单提交的方式 一般用post-->
   <!--表单控件介绍：
   name：向服务端传递数据的属性
   value:表示当前控件的值
   type:控件类型（文本、密码、单选、复选、邮箱）-->
   
   <form action="lesson05_图片.html" method="get">		
       用户名：<input type="text" name="username" id="i_name" placeholder="请输入账号"/><br />
   
       <!--placeholder:水印-->
       密码：<input type="password" name="password" placeholder="请输入密码"/><br />
       重复密码：<input type="password" name="repass" /><br />
       邮箱：<input type="email" size="10" /><br />
   
       <!--radio:单选框  name相同才能达到单选的目的 checked:默认-->
       性别：男<input type="radio" name="sex" value="男"/>女<input type="radio" name="sex" value="女"/><br />
       婚否：已婚<input type="radio" name="marry" value="已婚"/> 未婚<input type="radio" name="marry" value="未婚" checked/><br />
   
       <!--checkbox：复选框-->
       兴趣爱好：<br />
       打游戏：<input type="checkbox" name="like" value="打游戏"/>
       敲代码：<input type="checkbox" name="like" checked>
       <br />
   
       <!--select:下拉列表-->
       选择籍贯：<br />
       <select>
           <option>湖南省</option>
           <option>湖北省</option>
       </select>
       <br />
   
       选择所在区域：<br />
       <select>
           <optgroup label="湖北省">
               <option>武汉市</option>
               <option>黄冈市</option>
           </optgroup>
           <optgroup label="湖南省">
               <option>长沙市</option>
               <option>株洲市</option>
           </optgroup>
       </select>
   
       请选择所在专业：<br />
       <input type="text" list="majors"/>
       <datalist id="majors">
           <option>计算机应用与科学</option>
           <option>网络工程</option>
       </datalist>
       <br />
   
       请选择所精通的领域：<br />
       <!--multiple:多选-->
       <select multiple style="height: 100px; width:200px ;">
           <option>文学</option>
           <option>计算机操作</option>
       </select>
   
       <br />
       <!--textarea:文本域 -->
       自我介绍：<br />
       <textarea cols="40" rows="10"></textarea>
       <br />
   
       <!--submit自带触发时间，button不带-->
       <input type="submit" value="提交"/>
       <input type="button" value="提交"/>
   ```

9. HTML-分区标签  div/span

   * 块级元素/行内元素

   ```html
   <!--块级元素：独占一行 跟css结合使用-->
   <div style="background-color:red">
       这是一层
   </div>
   <div>
       这是一层
   </div>
   
   <!--行类元素：在同一行里面显示-->
   <span style="border:1px solid #000">111111</span>
   <span>222222</span>
   ```

10. HTML-语义化标签

   * header:表示头部块
   * nav:表示导航块
   * section：表示独立块
   * article:表示一篇文章
   * aside:表示一个侧边栏
   * footer:表示底部内容
   * dialog:表示一个对话框，默认隐藏状态，需要设置open属性

   