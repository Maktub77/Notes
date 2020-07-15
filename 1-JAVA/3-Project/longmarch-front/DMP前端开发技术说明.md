# dmp-front

> 大数据试验场数据管理平台web前端工程

## 开发须知 ##
* 环境搭建: nodejs 8.x.x 以上
* IDE: webstrom
* 前端核心技术: vue [https://cn.vuejs.org](https://cn.vuejs.org)
* 前端组件框架: element [http://element-cn.eleme.io/#/zh-CN](http://element-cn.eleme.io/#/zh-CN)

-----
> * 1: 后续开发，需要熟悉并掌握vue的技术栈，需要了解vue中的组件与路由概念。
> * 2: element是饿了么开放的自定义的组件库，后续开发可以直接找到对应的组件进行集成
> * 3: 项目中 /src/components/page/MyData.vue 有简单的element集成示例，简单的http调用示例
> * 4: 项目中 /src/components/router/index.js 是已有的界面跳转配置


-----

## 目录结构介绍 ##

	|-- build                            // webpack配置文件
	|-- config                           // 项目打包路径以及http调用路径
	|-- src                              // 源码目录
	|-- |-- assets                       // 静态文件
	|       |-- conf                     // 暂不使用
	|       |-- css                      // css文件与font文件
	|       |-- img                      // 图像
	|       |-- js                       // js文件
	|   |-- components                   // 组件
	|       |-- common                   // 自定义的与第三方的公共组件
	|           |-- Breadcrumb.vue       // 自定义的面包屑组件
	|       |-- structure                // 主界面的框架
	|           |-- ContentArea.vue      // 主要页面的展示区
	|           |-- SideBar.vue          // 侧边栏
	|           |-- TopBar.vue           // 顶部导航条
	|		|-- page                   	 // 主要页面
	|           |-- Home.vue             // 主页
	|           |-- Login.vue            // 登录界面
	|           |-- MyData.vue           // 我的数据界面
	|		|-- router                   // 路由
	|           |-- bus.js               // 用来通过路由传递组件间的数据，可不看
	|           |-- index.js             // 主要页面的跳转逻辑控制
	|   |-- App.vue                      // 页面入口文件
	|   |-- main.js                      // 程序入口文件，加载各种公共组件
	|-- static                           // 可忽略
	|-- test                             // 单元测试目录
	|-- theme                            // element-ui的主题包，由el命令生成，可忽略
	|-- .babelrc                         // ES6语法编译配置
	|-- .editorconfig                    // 代码编写规格
	|-- .gitignore                       // 忽略的文件
	|-- element-variables.scss           // element-ui的主题配置包，theme文件夹就是通过el命令由此文件中的配置生成
	|-- index.html                       // 入口html文件
	|-- package.json                     // 项目及工具的依赖配置文件
	|-- README.md                        // 说明



## 安装搭建

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8089
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run all tests
npm test
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
