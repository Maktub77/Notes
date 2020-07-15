## 常见CSS样式

### 背景

1. background-color设置背景颜色

   这个地的方颜色值接收三种形式

2. background-image设置背景图像

   注意：这个地方的图睛路径我们需要通过`url(图片路径)`这种写法包裹起来

3. background-size设置背景图大小

   * cover 拉伸图片，以长边为主
   * contain 但伸图片，以短边为主
   * `background-size: 100% 100%;` 宽与高设置成100%大小

4. background-repeat设置背景图重复排列试

   上面的排列可以赋值repeat-x，repeat-y以及no-repeat分别代表横向重复与纵重复以及不重复

   * background-repeat-x
   * background-repeat-y

   > 上面的两个属性有兼容性，要在IE8以上才能使用

5. background-position设置背景图定位

   设置背景图的定位 
   前面的参数代表横向，后面的参数代表纵向
   它可以使用`left/center/right    top/center/bottom`
   同时也可以使用具体的像素

   `background-position: 150px 200px;`

### 边框

1. border:1px solid black

   上面的border设置了线条的宽度为1px,线条的类型为solid（实线），线条的颜色为black(黑色)

2. border-style 设置线条的类型

   在这里，我们有几个常用的线条类型  solid实线,dotted紧凑型虚线,dashed虚线，double双线......

3. border-color设置线条的颜色，这个颜色接收三种值

4. border-width设置线条的宽度

5. border-top/right/bottom/left-style/width/color 

   分不同的方向，不同的类型去设置边框

   这一种组合方式有12个

6. border-raduis圆角边框

   通过这个属性，我们可以设置一个元素的圆角边框，如果设置的这个值大于或等于它的宽度一半，这个元素就会呈现出一个圆形

### 文字与字体

1. color设置文字的颜色，它接收三种颜色值

2. **text-align**设置文字的排列方向，它有三个值 `left/center/right`  

3. font-size设置字体大小，它接收几个值

   * px像素单位  网页上面默认的字体大小是16个像素单位
   * pt字号单位   网页上面默认的字体大小是12pt大小，它代表标准字体大小

   > 说明：12pt=16px

   * vw单位，这是一个响应式的单位，viewport width

     1vw代表设备宽度*1%

4. font-family设置字体类型

   后面跟一个字体名，这个字体名可以是中文，但是尽量不要写中文，要用英文

   例如"微软雅黑"要换成`Microsoft yahei`;

5. @font-face第三方字体

   我们可以使用@font-face这个命令来定义我们的一个自定义字体（第三方字体），具体使用如下

   **定义字体**

   ```css
   @font-face {
       font-family:'fzkt';
       src: url(fonts/fzkt.ttf);
   }
   ```

   > 说明：上面的@font-face是自定义定体的命令，里面的font-family代表这个自定义定体的名子，后面跟s着的src代表这个字体的位置

   **使用字体**

   ```css
   .p1{
       text-align: center;
       font-family: 'fzkt';
   }
   ```

   > 说明：这个时候的font-family就引入了上面刚刚定义好的自定义定体名`fzkt`，效果图如下所示

   ![1529652999311](assets/1529652999311.png)

6. font-weight设置字体的权重

   它有常用的三个属性值lighter/normal/bolder,`font-weight:bolder`要当于把字体加粗了

7. text-decoration设置文字的划线方式

   它常设置的值有三个underline下划线，overline上划线以及line-through删除线（相当于del或strike标签的效果）

8. text-shadow文字阴影效果

   它可以设置文字的阴影响果，它的属性值有四个

   第一个值代表横向偏移，如果是负值就代表向左偏移，如果是正值则代表向右偏移

   第二个值代表纵向偏移，如果是负值则代表向上偏移，如果是正值则代表向下偏移

   第三个值代表阴影模糊度，值越大阴影越模糊，最小值为0即没有模糊度

   第四个值代表阴影的颜色值，它接收三种类型颜色

9. text-indent首行缩进

   `text-indent:2em`这句代码的意思就代表首行缩进两个字符位

   它不仅仅可以使用em字符单位，还可以使用像素px做单位如`text-indent:50px`代表首行缩进50px

10. text-transform字体变幻

    * uppercase把英文字体变成大写
    * lowercase 把英文字母变成小写
    * capitalize 每个单词的首字母变成大写

11. letter-spacing字体之间的间距，接收一个像素值

12. line-height设置字体的高行，这个行高接收一个数字单位，同时也可以接收一个像素单位

    **注意**：在接收像素单位的时候，我们可以通过这个特性来实现文字的上下排列方式，例如上下居中

    ```css
    .div1{
        width: 200px;
        height: 100px;
        border: 1px solid black;
        text-align: center;   /*实现了文字的左右居中*/
        line-height: 100px;
    }
    ```

    > **说明**：上面的代码里面，我们的行高设置成了100px,同时，我们的div1的高度也设置成了100px.这个时候，行高与高度相同，就会出现文字在这个div1里面呈现上下居中的效果

13. （重点）文字排列的综合应用

    * white-space设置文字是否换行nowrap/wrap
    * overflow 设置超出的部分怎么处理
    * text-overflow: ellipsis; 设置超出的文字有“···”去替代

    ![1529655095183](assets/1529655095183.png)

14. direction文字的书写方向，常用的值有ltr(从左向右)以及rtl(从右向左)

### 列表样式

1. list-style-type设置前面的列表导航类型

   如果导航的类型设置成了none则代表去掉前面的导航图标

2. ul是一个非常特殊的标签，它自带外边框与内边框

3. 外边距

4. 内边距

