### 第三方接口调用封装

* 基于公司项目情况，几乎每个项目都有大量的调用第三方数据接口的情况，因此急需一个公共的调用第三方接口的组件。同时也需要在调用过程中对于缓存、出错日志、重试等公共操作的封装。 

#### 组件设计说明

1. 代码简洁、清晰
2. 面向接口编程，支持扩展
3. 职责单一原则
4. 组件架构如下图：![img](https://img-blog.csdn.net/2018073015172662?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eWkxOTg3MTIxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)