### jQuery的DOM操作

* 使用jQuery的九种选择器可以基本选中需要操作的对象，但是为了提高jQuery的查询效率，可以结合jQuery的内置查找函数一起使用

#### 1.查询

* `children([expr])` 获取指定的子元素
* `find(expr) `获取指定的后代元素
* `parents([expr]) `获得祖辈元素
* `parent() `获取父元素
* `next([expr]) `获取下一个兄弟元素
* `prev([expr])` 获取前一个兄弟元素
* `siblings([expr]) `获取所有兄弟元素
* XML解析中find方法使用最多
* 对查找结果进行遍历操作 each(function(){… }) ， 在each函数中可以通过this 获得DOM对象， $(this) 获得jQuery对象

#### 2.属性操作

* 设置属性 `attr(name,value)`
* 读取属性 `attr(name)`
* 同时设置多个属性` attr({name:value,name:value... })`
* `  attr("checked","true") `等价于` prop("checked")`

#### 3.CSS操作

* 通过attr属性设置/获取style属性

  ```javascript
  attr('style','color:red'); // 添加style属性
  ```

* 设置CSS样式属性css(name,value)设置一个CSS样式属性

  ```javascript
  css(properties) //传递key-value对象， 设置多个CSS样式属性
  ```

* 设置class属性

  * `addClass(class) `添加一个class属性
  * `removeClass([class]) `移除一个class属性
  * `toggleClass(class)`如果存在（ 不存在） 就删除（ 添加） 一个类

#### 4.HTML代码&文本&值操作

* 读取和设置某个元素中HTML内容
  * html() 读取innerHTML
  * html(content) 设置innerHTML
* 读取和设置某个元素中的文本内容
  * text() 读取文本内容
  * text(content) 设置文本内容
* 文本框、下拉列表框、单选框 选中的元素值
  * val() 读取元素value属性
  * val(content) 设置元素value属性

#### 5.jQuery添加元素

* 创建元素
  * 拼接好HTML代码片段$(HTML片段)---产生jQuery对象
* 内部插入：
  * `$node.append($newNode) `内部结尾追加
  * `$node.prepend($newNode) `内部开始位置追加
* 外部插入：
  * `$node.after($newNode) `在存在元素后面追加 -- 兄弟
  * `$newNode.insertBefore($node)` 在存在元素前面追加

#### 6.jQuery删除元素

* 选中要删除元素.remove() ----完成元素删除
* 选中要删除元素.remove(expr) -----删除特定规则元素
* remove删除节点后，事件也会删除
* detach删除节点后，事件会保留

#### 7.jQuery复制和替换

* 复制节点
  * `$(“p”).clone();` 返回节点克隆后的副本， 但不会克隆原节点的
  * `$(“p”).clone(true);` 克隆节点， 保留原有事件
* 替换节点
  * `$("p").replaceWith("<b>softeem</b>"); `将所有p元素， 替换为`"<b>softeem</b>“`
  * `$(“<b>softeem</b>”).replaceAll(“p”); `与replaceWith相反

