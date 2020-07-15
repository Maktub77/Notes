##### v-if和v-show

1. 手段：v-if是动态的向DOM树内添加或者删除DOM元素；v-show是通过设置DOM元素的display样式属性控制显隐
2. 编译过程：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重构内部的时间监听和子组件；v-show只是简单的基于css切换
3. 编译条件：v-if 是惰性的，如果条件为假，则什么也不做；只有条件第一次变为真时才开始局部编译（编译被缓存后，然后再切换的时候进行局部卸载）；v-show是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且DOM元素保留
4. 性能消耗：v-if有更高的切换消耗；v-show有更高的初始渲染消耗
5. 使用场景：v-if适合运营条件不大可能改变；v-show适合频繁切换

##### 数组与对象互转

* 数组转对象

  `Object.assign( {},  self.taskOrderAddform)`

* 对象转数组

  `Object.values(self.taskOrderAddform)`

##### Vue.set()

* 调用方法：`Vue.set(target,key,value)`
  * target：要更改的数据源（可以是对象或者数组）
  * key：要更改的具体数据
  * value：重新赋的值
* 可以修改，删除对象或数组中的对象

> Tip:`Vue.set()`在methods中可以写成`this.$set()`

##### forEach，map

1. 都是用来遍历数组的方法
2. 两个函数都有4个参数item(当前项)，index(当前项的索引)，arr(原数组)，还有一个可选参数this
3. 匿名函数中this默认是指向window的
4. 对空数组不会调用回调函数
5. 不会改变原数组(某些情况下可改变)

###### forEach

* 没有返回值

  ```vue
  var a = [1,2,3,4,5]
  var b = a.forEach((item) => {
      item = item * 2
  })
  console.log(b)
  // undefiined
  ```

###### map

* 返回一个经过处理后的新数组，但不改变原数组的值

  ```vue
  var a = [1,2,3,4,5]
  var b = a.map((item) => {
  	return item = item*2
  })
  console.log(a)  // [1,2,3,4,5]
  console.log(b)  // [2,4,6,8,10]
  ```

##### v-for下的表单验证

1. 添加rules规则，和正常添加规则一样

   ```vue
   rules: {
   	userId:{required: true, message: "用户名不能为空", trigger: "blur"},
   	stepValuesName:{
           required: true, message: "stepValuesName不能为空", trigger: "blur"
       },
   	isDistribute:{required: true, message: "请选择是否分配", trigger: "change"},
   },
   ```

2. 给表单添加验证

   ```VUE
   <el-row>
       <el-col :span="18">
           <el-form-item label="默认用户Id"  :prop="'taskOrder.'+ index +'.userId'" :rules="rules.userId">
               <el-input placeholder="多个用户用逗号隔开" v-model="item.userId" class="col-sm-12"></el-input>
           </el-form-item>
       </el-col>
   </el-row>
   ```

   > 注：这里每个表单都要添加`:rules`，多个用`rules.v-modle的名称`区分

   > `:prop`正常验证表单项`prop`改为`:prop`
   >
   > `:prop="'taskOrder.'+ index +'.userId'"`是拼接形式，前面是v-for绑定的数组，中间是数组索引index，最后是表单项绑定的v-model的名称，然后用.连接起来

   ```vue
   <el-row>
       <el-col :span="18">
           <el-form-item label="是否可以选择处理人"
                         :prop="'taskOrder.'+ index +'.taskTargetRefList.' + key + '.isDistribute'"
                         :rules="rules.isDistribute">
               <el-radio-group v-model="option.isDistribute" >
                   <el-radio label="true">是</el-radio>
                   <el-radio label="false">否</el-radio>
               </el-radio-group>
           </el-form-item>
       </el-col>
   </el-row>
   ```

   当有双层嵌套时同理。

3. v-for绑定的数组也必须绑定在form对象里 

   ```vue
   taskOrderAddform:{
   	taskOrder:[{
           userId:"",
           taskTargetRefList:[{
               isDistribute:"",
               stepValuesName:""
   		}]
   	}]
   },
   ```

