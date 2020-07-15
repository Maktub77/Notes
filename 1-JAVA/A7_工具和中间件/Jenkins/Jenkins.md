<https://www.bilibili.com/video/BV1e4411U7QE?p=5> 

持续部署

持续集成

持续交付

* 降低风险
* 减少重复的过程
* 任何时间 任何地点生成可部署的软件
* 增加项目的可见性
* 建立团队对开发产品的信心

持续集成工具

* Jenkins和Hudson
  * 可以整合GitHub或Subversion

JavaEE项目部署方式对比

* 手动部署

  ![1586480766546](C:\Users\Maktub\Documents\Notes\0-picture\Jenkins\手动部署.png)

* 自动化部署

  ![1586480787116](C:\Users\Maktub\Documents\Notes\0-picture\Jenkins\自动化部署.png)

前置知识

* Linux基本操作命令和vim编辑器使用
* Maven的项目构建管理
* GitHub或SVN使用

Jenkins+SVN持续集成环境搭建

* 安装Linux系统
  * SVN版本库+Jenkins+项目运行服务器
* 版本控制子系统
  * Subversion服务器
  * 项目对应版本库
  * 版本中的钩子程序

Jenkins+GitHub持续集成环境搭建

http://10.241.39.17/CDIB/cuteinfo5.git

* Jenkins要部署在外网上，因为内网地址Github是无法访问的。这一点可以通过租用阿里云等平台提供的云服务器实现
* Jenkins所在的主机上需要安装Git，通过Git程序从Github上clone代码
* 在Jenkins内需要指定Git程序的位置，和指定JDK，Maven程序位置位置非常类似
* 在GitHub上使用每个repository的WebHook方式远程触发Jenkins构建
* 在Jenkins内关闭"防止跨站点请求伪造"

target 编译和打包结果