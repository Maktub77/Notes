### Windows分区和Linux分区的区别

* 在Windows操作系统中，是将物理地址分开（分出主分区和逻辑分区），再在分区上建立目录。在Windows操作系统中，所有的路径都是从盘符开始：c://...
* Linux正好相反，是先有目录，再将物理地址（分区）映射到目录中。在linux操作系统中，所有路径都是从根目录开始。**Linux默认3个分区，分别是boot分区。swap分区和根分区。**

1. **/boot区**，通常情况下根据Linux的版本不同，个人分区习惯会不同，我这里分配了500M给这个分区。

2. **swap区**，交换区，通常分配给其的大小为物理内存的2倍，但是最好不要超过256M，所以我这里分配了256M给这个分区。

3. **/ 区**，也就是根目录，这个分区尽量给其分配大的空间，可以将安装Linux系统的这个硬盘上除去分给/boot、swap区以外的空间都分配给这个分区。

sudo是普通用户想以root身份运行命令

yum是管理软件安装、卸载、升级的命令工具

linux如何卸载自带的openJdk，并且安装jdk1.8

* ```vhdl
  1.查询OpenJDK
  # rpm -qa|grep java
  2.删除openJDK版本
  [root@mini01 dupenghui]# rpm -e --nodeps openJDK版本
  3.再次查询OpenJDK，openJDK已删除
  4.解压 #如果需要解压到其他目录 可以在末尾处加上 -C /usr/local/
  tar -zxvf jdk-7u67-linux-i586.tar.gz
  5.配置环境变量，输入命令
  vi /etc/profile 
  最后加上：
  JAVA_HOME=/usr/java/jdk1.8.0_101
  JRE_HOME=/usr/java/jdk1.8.0_101/jre
  PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
  CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
  export JAVA_HOME JRE_HOME PATH CLASSPATH
  6. 刷新配置，输入命令
  source /etc/profile   //使修改立即生效 
  7.查看配置是否生效
  echo $PATH
  ```

