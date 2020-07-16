https://blog.csdn.net/peter_teng/article/details/78617257

备份数据库到远程主机

6.备份数据库到远程主机

`scp d4tool_$(date +%Y%m%d_%H%M%S).sql root@72.191.254.261:/home/backup `

意思是将本地文件复制到261这个主机的backup目录下，scp命令参考如下博客
scp命令

这样每次用scp命令的时候都需要输入261的登陆密码，很不方便，需要免密输入

7.scp无需输密码传送文件

两种方法：1.配对秘钥 2.用expect脚本

* 配对秘钥
  在源主机A上用如下命令生成配对秘钥：
  [root@app ~]# ssh-keygen -t rsa 
  Generating public/private rsa key pair.
  Enter file in which to save the key (/root/.ssh/id_rsa): 
  Enter passphrase (empty for no passphrase): 
  Enter same passphrase again: 
  Your identification has been saved in /root/.ssh/id_rsa.
  Your public key has been saved in /root/.ssh/id_rsa.pub.
  The key fingerprint is:
  e2:9e:98:7a:36:62:bc:d8:d1:b3:3c:fe:f8:4e:fd:9c root@app.web.140.242.189.217.com
  The key's randomart image is:
  +--[ RSA 2048]----+
  |                 |
  |                 |
  |                 |
  |                 |
  |      . S        |
  |   . . o         |
  | .. o o .        |
  | o+o=O . o .     |
  |..+BB**   E      |
  +-----------------+
  直接enter就好，生成的rsa跟pub文件都放在默认的/root/.ssh目录下。将pub文件拷贝到目标主机B上的/root/.ssh/authorized_keys中（如果没有则创建一个），
  scp id_rsa.pub root@427.931.244.211:/root/.ssh/authorized_keys 

* 用expect脚本

  #!/usr/bin/expect  
  set timeout 10  
  set host [lindex $argv 0]  
  set username [lindex $argv 1]  
  set password [lindex $argv 2]  
  set src_file [lindex $argv 3]  
  set dest_file [lindex $argv 4]  
  spawn scp $src_file $username@$host:$dest_file  
   expect {  
   "(yes/no)?"  
    {  
  ​    send "yes\n"  
  ​    expect "*assword:" { send "$password\n"}  
    }  
   "*assword:"  
    {  
  ​    send "$password\n"  
    }  
  }  
  expect "100%"  
  expect eof 

  注意代码刚开始的第一行，指定了expect的路径，与shell脚本相同，这一句指定了程序在执行时到哪里去寻找相应的启动程序。代码刚开始还设定了timeout的时间为10秒，如果在执行scp任务时遇到了代码中没有指定的异常，则在等待10秒后该脚本的执行会自动终止。

  从以上代码刚开始的几行可以看出，我为这个脚本设置了5个需要手动输入的参数，分别为：目标主机的IP、用户名、密码、本地文件路径、目标主机中的文件路径。如果将以上脚本保存为expect_scp文件，则在shell下执行时需要按以下的规范来输入命令：

  ./expect_scp 192.168.75.130 root 123456 /root/src_file /root/dest_file  

  以上的命令执行后，将把本地/root目录下的src_file文件拷贝到用户名为root，密码为123456的主机192.168.75.130中的/root下，同时还将这个源文件重命名为dest_file
  spawn代表在本地终端执行的语句，在该语句开始执行后，expect开始捕获终端的输出信息，然后做出对应的操作。expect代码中的捕获的(yes/no)内容用于完成第一次访问目标主机时保存密钥的操作。有了这一句，scp的任务减少了中断的情况。代码结尾的expect eof与spawn对应，表示捕获终端输出信息的终止

  使用expect需要了解的一点是：用expect速度会比较慢，因为需要等待返回的数据，然后输入命令执行，没有ssh密钥登录的快速
