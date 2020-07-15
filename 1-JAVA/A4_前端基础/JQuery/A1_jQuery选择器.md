###  jQuery九种选择器

* 选择器是jQuery的根基，在jQuery中，对事件处理，遍历DOM和Ajax操作都依赖于选择器
* jQuery(expression,[context])在核心函数jQuery中传入表达式，对页面中元素进行选择-------------jQuery核心函数第二种

#### 1.基本选择器

* 根据元素id属性、class属性、元素名称
  * id选择器：$("#元素id属性")
  * class选择器：$(".元素class属性")
  * 元素名称选择器：$("元素名称")
* 多个选择器同时使用selector 1,selector 2 例如：
  * `$("#xxid,.xxxclass")`同时选择id和class匹配两类元素

#### 2.层级选择器

* 根据祖先、后代、父子关系、兄弟关系 进行选择
  * ancestor descendant 获取ancestor元素下边的所有元素
  * parent > child 获取parent元素下边的所有直接child子元素
  * prev+next 获取紧跟prev元素的后一个兄弟元素
  * prev~siblings 获取prev元素后边的所有兄弟元素

#### 3.基本过滤选择器

* :first 选取第一个元素 $("tr:first")
* :last 选取最后一个元素 $("tr:last")
*  :not(selector) 去除所有与给定选择器匹配的元素 $("input:not(:checked)")
* :even 选取所有元素中偶数索引的元素， 从 0 开始计数 $("tr:even") ----- 选取奇数元素
* :odd 选取所有元素中奇数索引的元素 ， 从0 开始计数 $("tr:odd") ------ 选取偶数元素
* :eq(index) 选取指定索引的元素 $("tr:eq(1)")
* :gt(index) 选取索引大于指定index的元素 $("tr:gt(0)")
* :lt(index) 选取索引小于指定index的元素 $("tr:lt(2)")
* :header 选取所有的标题元素 如： h1, h2, h3 $(":header")
* :animated 匹配所有正在执行动画效果的元素

#### 4.内容过滤选择器

* 内容选择器是子元素和文本内容的操作
* :contains(text) 选取包含text文本内容的元素 `$("div:contains('John')") `文本内容含有john 的所有div
* :empty 选取不包含子元素或者文本节点的空元素` $("td:empty") td`元素必须为空
* :has(selector) 选取含有选择器所匹配的元素的元素` $("div:has(p)").addClass("test");` 含有p子元素的div
* :parent 选取含有子元素或文本节点的元素 `$("td:parent") `所有不为空td元素选中

#### 5.可见性过滤选择器

* 根据元素的可见于不可见状态来选取元素
  * :hidden 选取所有不可见元素 $("tr:hidden")
  	 :visible 选取所有可见的元素 $("tr:visible")	
* CSS
  * visibility: hidden 保留自己的位置
  * display:none  不保留自己的位置

#### 6.属性过滤选择器

* 通过元素的属性来选取相应的元素
  * [attribute] 选取拥有此属性的元素 $("div[id]")
  * [attribute=value] 选取指定属性值为value的所有元素
  * [attribute !=value] 选取属性值不为value的所有元素
  * [attribute ^= value] 选取属性值以value开始的所有元素
  * [attribute $= value] 选取属性值以value结束的所有元素
  * [attribute *= value] 选取属性值包含value的所有元素

#### 7.子元素过滤选择器

* 对某远素中的子元素进行选取
  * :nth-child(index/even/odd) 选取索引为index的元素、 索引为偶数的元素、 索引为奇数的元素 ----- index 从1开始 区别 eq
  * :first-child 选取第一个子元素
  * :last-child 选取最后一个子元素
  * :only-child 选取唯一子元素， 它的父元素只有它这一个子元素

#### 8.表单过滤选择器

* 选取表单的过滤选择器
  * :input 选取所有<input>、 <textarea>、 <select >和<button>元素
  * :text 选取所有的文本框元素
  * :password 选取所有的密码框元素
  * :radio 选取所有的单选框元素
  * :checkbox 选取所有的多选框元素
  * :submit 选取所有的提交按钮元素
  * :image 选取所有的图像按钮元素
  * :reset 选取所有重置按钮元素
  * :button 选取所有按钮元素
  * :file 选取所有文件上传域元素
  * :hidden 选取所有不可见元素

#### 9.表单对象属性过滤选择器

* 选取表单元素属性的过滤选择器
  * :enabled 选取所有可用元素
  * :disabled 选取所有不可用元素
  * :checked 选取所有被选中的元素， 如单选框、 复选框
  * :selected 选取所有被选中项元素， 如下拉列表框、 列表框

### jQuery选择器总结

1. 对象访问核心方法                                                                                                                                                              
   * each(function(){}) 遍历集合
   * size()/length属性 返回集合长度
   * index() 查找目标元素是集合中第几个元素
2. CSS样式操作
   * css(name,value) 

     css({name:value,name:value}); 同时修改多个CSS样式

   * 基本过滤选择器与 筛选过滤 API功能是相同

   * `$("tr:first")` 等价于 `$("tr").first()`
3. 九种选择器重点
   * 基本选择器和层级选择器 锁定元素

   * 使用属性过滤选择器和内容过滤选择器 具体选中元素

   * 表单操作 

     :checked 

     :selected 选中 表单选中元素

   * 配合基本过滤选择器， 缩小选中的范围