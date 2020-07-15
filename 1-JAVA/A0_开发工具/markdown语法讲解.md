## markdown语法讲解 二号标题

### 标题类 三号标题

#### 标题的区分

#### 标题的书写

### 代码类

### 图片类

### 表格类

-------

1. 第一项

   我可以在这里写其它的东西

2. 第二项

   * 无序列表嵌套
   * 无序列表嵌套2

3. 第三项

-------

* 无序列表一
  * 二级无序列表
* 无序列表二

----

- 减号可以

------

+ 加号也可以

------------------

## 列表的嵌套

* 男生
  * 深哥
  * 强哥
    * 小强哥
    * 大强哥
  * 标哥
* 女生
  * 小娇
  * 张芳

---

## markdown代码高亮

如果在markdown里面要插入代码笔记，只需 要如下设置

```css
.div2,.div3{
    width: 100px;
    height: 50px;
    float: left;
}
```

例如，我们现在要定这里插入html的代码

```html
<body>
    <div class="div1">
        <div class="div2"></div>
        <div class="div3"></div>
    </div>
    <div class="div4"></div>
</body>
```

块状的代码

```csharp
using System;
using System.Text;
using System.Collections;
using System.Collections.Generic;

namespace getYouKuDataDemo
{
    //扩展方法
    public static class ExtendMethods
    {
        //指定字符串分割
        public static string Join(this IEnumerable list,string pattern)
        {
            string text=string.Empty;
            foreach(var item in list)
            {
                text+=pattern+item;
            }
            return text.Substring(pattern.Length);
        }
    }
}
```

> 说明：上面的代码是自定的义的扩展方法，用于xxxxxxx

*行内的代码，行内的代码不用标题，直接前后各给一个点就可以了*

css中设置一行背景色为红色的代码是`background-color:red`



## markdown中的引用说明

> 这是一段引用说明文档

## markdown中特殊字体设置

* 字体倾斜 *这里的文字是倾斜的*
* 字体加粗  **这里是加粗的字体**
* 字体删除 ~~这里面是删除的字体~~
* 下划线字 <u>这里是下划线的字体</u>

## markdown中的任务列表（多选框）

*空格[空格]空格   内容

* [ ] 内容

*空格[x]空格 内容

* [x] 选中的

## markdown中的表格

| 表头   | 表头       | 表头       |
| ------ | ---------- | ---------- |
| 内容   | 第一行内容 | 第一行内容 |
| 第二行 | 第二行     | 第二行     |
| 第三行 | 第三行     | 第三行     |

## markdown中的图标

向左的箭头 :arrow_left:

向右的箭头 :arrow_right:

:arrow_up: :arrow_down:

数字图标

:one: :two: :three: :four: :five: :six: :seven: :eight: :nine: :keycap_ten:

像形图标

:banana: :watch: :watermelon: :information_desk_person: :girl: :boy: :monkey:

## markdown的网页连接

[百度一下](http://www.baidu.com)

[百度一下]:http://www.baidu.com

## markdown中插入图片

在markdown里面插入图片不是很好弄，一般建议插入网络图片

![](http://192.168.0.254/uploads/-/system/appearance/header_logo/1/logo.png)

如果要插入本地图片

![](assets/g.jpg)

![]()