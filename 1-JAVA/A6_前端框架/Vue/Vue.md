## Vue

* 是一个构建数据驱动的web界面的库。Vue.js的目标是通过尽可能简单的API实现**响应的数据绑定**和**组合的视图组件**
* Vue.js自身不是一个全能框架--它只聚焦于视图层。

### vue-cli

* vue-cli是vue.js的脚手架，用于自动生成vue.js+webpack的项目模板。

#### 使用

1. 我们首先我们要建一个储存webstorm项目的文件夹，我的命名是webstormproject。

2. 进入到这个文件夹，右键，选择Git Bath Here。

3. 输入命令行：npm install vue-cli -g (下载全局vue-cli)

4. 等到下载完成后，输入命令行：vue init webpack myproject  （myproject是项目名，我的项目名就叫myproject）

   然后他会询问你一些问题：

5. Project name (myproject)；项目名称（myproject）。（确定按enter，否按N） 

6. Project description (A Vue.js project)；项目描述（一vue.js项目）。（随意输入一段简短介绍，用英文，不写直接回车也行） 

7. Author (sunsanfeng)；作者（sunsanfeng）。（确定按enter，否按N） 

8. Vue build (Use arrow keys)> Runtime + Compiler: recommended for most usersRuntime-only: about 6KB lighter min+gzip, but templates (or any Vue-specificHTML) are ONLY allowed in .vue files - render 　　　　　　　　functions are required elsewhere；Vue公司的建立（使用箭头键）>运行时+编译器：大多数用户推荐运行时间：约6kb轻民+ gzip，但模板（或任何Vue具体HTML）只允许在。VUE文件渲染功能是必需的其他地方。（按enter） 

9. Install vue-router? (Y/n)；安装的路由？（/ N）。（可安可不安，建议安装，因为项目肯定能用上） 

10. Use ESLint to lint your code? (Y/n)；使用ESlint语法？（Y/ N）。（使用ESLint语法，就要做好心理准备，除非你非常懂ESLint语法，要不就会处处报错，之前不明白的时候选择过一次，总之很烦，若想要挑战一下，下面这个网址会给你帮助的：https://cloud.tencent.com/developer/chapter/12618        建议N） 

11. Setup unit tests with Karma + Mocha? (Y/n)；设置单元测试？（Y / N）。（选N） 

12. Setup e2e tests with Nightwatch? (Y/n)；Nightwatch建立端到端的测试？（Y / N）。（选N）

13. should we run 'npm install' for you after the ogject has been created? ;(选择Yes，use NPM）

    **等待一会儿，项目就建好了**。

14. 输入命令：cd myproject  进入到项目文件中

15. 输入命令： npm install  初始化安装依赖   

16. 输入命令： npm run dev  执行

---

### [单文件组件](https://www.cnblogs.com/yangguoe/p/9063912.html)（single-file components）

1. .vue文件

   * .vue文件，称为单文件组件，是Vue.js自定义的一种文件格式，一个.vue文件就是一个单独的组件，在文件内封装了组件相关的代码：html、css、js

     ```vue
     <!--.vue文件由三部分组成：<template>、<style>、<script>-->
     <template>
     	html
     </template>
     <style>
         css
     </style>
     <script>
         js
     </script>
     ```

2. vue-loader

   *  浏览器本身并不认为.vue文件，所以必须对.vue文件进行加载解析，此时需要vue-loader
   * 类似的loader还有许多，如：html-loader、css-loader、style-loader、babel-loader等
   * 需要注意的是vue-loader是基于webpack的

3. webpack

   *  webpack是一个前端资源模板化加载器和打包工具，它能够把各种资源都作为模块来使用和处理
   * 实际上，webpack是通过不同的loader将这些资源加载后打包，然后输出打包后文件 
   * 简单来说，webpack就是一个模块加载器，所有资源都可以作为模块来加载，最后打包输出
   * [官网](http://webpack.github.io/)     
     ​    webpack版本：v1.x v2.x
     ​    webpack有一个核心配置文件：webpack.config.js，必须放在项目根目录下

4. 示例，步骤：

   1. 创建项目，目录结构 如下：
      webpack-demo
      ​    |-index.html
      ​    |-main.js   入口文件
      ​    |-App.vue   vue文件 
      ​    |-package.json  工程文件
      ​    |-webpack.config.js  webpack配置文件
      ​    |-.babelrc   Babel配置文件

   2. 编写App.vue

   3. 安装相关模板    

   4. 编写main.js

      ````js
      //使用ES6语法引入模块
      import Vue from 'vue'
      import App from './App.vue'//引入自定义模块，当前目录下要加./
      new Vue({
          el:'#app',
          renter:function(h){
              return h(App);
          }
      })
      ````

   5. 编写webpack.config.js

      ````js
      module.exports={
          entry:'./main.js',//入口文件
          output:{
              path:__dirname,//项目根路径
              filename:'bundle.js'//打包后的文件，需要手动引入到index.js中
          },
          // 配置模块加载器
          module:{
              rules:[
                  {
                      test:/\.vue$/,
                      loader:'vue-loader'
                  },
                  {
                      test:/\.js$/,
                      loader:'babel-loader',
                      exclude:/node_modules/
                  }
              ]
          }
      }
      ````

   6. 编写.babelrc 

   7. 编写package.json

   8. 运行测试
      ​    npm run dev



