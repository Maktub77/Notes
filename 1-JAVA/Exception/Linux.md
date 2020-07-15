#### 如何解决linux报No space left on device错误

##### 发现问题

出现这个错误第一反应是空间满了。

df -h 一看却发现还有挺多没有用 
df -i 一看发现是inodes空间满了

##### 解决方法

1. 删除掉没用的临时文件，释放inodes

   可以到/tep目录下看看有没有很多sess_xxxx的session临时文件

   ls -lt /tmp | wc -l

   如果发现文件特别多，则：

   find /tmp -type f -exec rm {} \;

2. 0字节的文件也会占用一个inode，也必须删除掉

   遍历查找并删除

   find /home -type f -size 0 -exec rm {} \;

3. 遍历所有文件目录找出占空间大的文件，进行适当删除

   先遍历出来占的空间大的目录

   for i in /*; do echo \$i; find $i | wc -l; done

   （如果确定是某个目录下面，则/转换为该目录绝对路径，如/var/spool，则使用for i in /var/spool/*; do echo \$i; find $i | wc -l; done）
   一般来看是/var/spool底下的邮件相关的特别大。

   find /var/spool/exim/msglog/ -type f -name ‘*’ -print0 | xargs -0 rm -rf

   find /var/spool/exim/input/ -type f -name ‘*’ -print0 | xargs -0 rm -rf

