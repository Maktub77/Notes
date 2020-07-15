#### Nginx安装（Linux）

1. 将nginx-1.12.2.tar.gz上传到服务器/usr/local目录下
2. cd到/usr/local目录下 执行tar -zxvf nginx-1.12.2.tar.gz解压安装包
3. cd到nginx-1.12.2目录下执行./configure命令（需要gcc环境，没有的话：yum -y install gcc gcc-c++）

* 如果报错：./configure: error: the HTTP rewrite module requires the PCRE library.     
  * 原因:服务器没有安装pcre-devel，zlib-devel，openssl-devel等依赖
  * 解决方法:  执行yum install pcre-devel  如果./不报错则执行yum install zlib zlib-devel，yum install openssl openssl-devel

* 如果再次报错：Could not retrieve mirrorlist http://mirrorlist.centos.org/?release=7&arch=x86_64&repo=os&infra=stock error was    

  * 原因：找不到镜像：服务器无法访问外网

  * 解决方法：本机下载pcre-devel，zlib-devel，openssl-devel包，手动上传到服务器/usr/local目录下并对这三个文件分别执行以下操作：

    1. tar -zxvf pcre-devel.tar.gz解压  
    2. cd pcre-devel 到目录   
    3. 执行./configure  
    4. 执行make
    5. 执行make install

    ------

    1. tar -zxvf zlib-devel.tar.gz解压  
    2. cd zlib-devel 到目录
    3. 执行./configure
    4. 执行make
    5. 执行make install

    ----

    1. tar -zxvf openssl-devel.tar.gz解压
    2. cd openssl-devel 到目录
    3. 执行./configure(注意：如果openssl-devel目录下的可执行文件是configure则执行./configure,如果openssl-devel目录下的可执行文件是config则执行./config)
    4. 执行make
    5. 执行make install

4. cd到/usr/local/nginx-1.12.2下执行以下操作
   1. ./configure
   2. make
   3. make install

5. 成功完成以上操作后/usr/local目录下会生成nginx目录
6. cd到/usr/local/nginx/sbin目录下  
   * 执行./nginx启动nginx
7. 测试nginx是否启动成功方法：
   * 执行netstat -nl|grep 80 查看80端口是否被监听
   * 浏览器直接访问服务器ip:80  看是否能进入nginx页面






