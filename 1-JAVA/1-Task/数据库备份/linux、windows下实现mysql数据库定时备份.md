### [Linux上定时备份MySQL数据库](https://www.cnblogs.com/pcyy/p/7238950.html)

* **MySQL的备份命令**：`mysqldump`  该命令可以将MySQL的数据库信息，通过SQL的方式存储在一个文件中。
* **crontab命令**：常见于Unix和类Unix的操作系统之中，用于设置周期性被执行的指令

1. 查看磁盘空间情况，选择一个空间充足的磁盘空间，避免出现因空间不足导致备份失败

   ```
   # df -h
   ```

2. 创建备份目录

   ```
   cd /home
   mkdir backup
   cd backup
   ```

3. 创建备份Shell脚本

   注意把以下命令中的DatabaseName换为实际的数据库名称；

   也可以使用其它的命名规则！

   ```
   vi bkDatabaseName.sh
   ```

   ```
   #!/bin/bash
   mysqldump -uusername -ppassword DatabaseName > /home/backup/DatabaseName_$(date +%Y%m%d_%H%M%S).sql
   ```

   输入上面的内容就可以了，下面对备份进行压缩是另一种方式，和上面的二选一即可，可以不考虑

   ---

   对备份进行压缩

   ```
   #!/bin/bash
   mysqldump -uusername -ppassword DatabaseName | gzip > /home/backup/DatabaseName_$(date +%Y%m%d_%H%M%S).sql.gz
   ```

   ---

   > 把 username 替换为实际的用户名； 
   > 把 password 替换为实际的密码； 
   > 把 DatabaseName 替换为实际的数据库名；

   退出编辑页：点击ESC退出，然后点击":wq"w写入write q推出quit

4. 添加可执行权限

   ```
   chmod u+x bkDatabaseName.sh
   ```

   添加可执行权限之后先执行一下，看看脚本有没有错误，能不能正常使用；

   ```
   ./bkDatabaseName.sh
   ```

5. 添加计划任务

   确认crontab是否安装： 

   ```
   # crontab
   ```

   * 执行 crontab 命令如果报 command not found，就表明没有安装
     * [CentOS下使用yum命令安装计划任务程序crontab](https://blog.csdn.net/testcs_dn/article/details/48780971)
     * [使用rpm命令从CentOS系统盘安装计划任务程序crontab](http://blog.csdn.net/testcs_dn/article/details/48781553)

   * 已安装

     * 执行命令

       ```
       crontab -e
       ```

       这时就像使用vi编辑器一样，可以对计划任务进行编辑。 
       输入以下内容并保存：

       ```
       */1 * * * * /home/backup/bkDatabaseName.sh
       ```

       具体含义：每一分钟执行一次shell脚本“/home/backup/bkDatabaseName.sh”。

       [Linux下的crontab定时执行任务命令详解](https://www.cnblogs.com/longjshz/p/5779215.html)

       ```
       在crontab文件中如何输入需要执行的命令和时间。
       该文件中每行都包括六个域，其中前五个域是指定命令被执行的时间，最后一个域是要被执行的命令。
       每个域之间使用空格或者制表符分隔。格式如下： 
       　　minute hour day-of-month month-of-year day-of-week commands 
           合法值 00-59 00-23 01-31 01-12 0-6 (0 is sunday) 
       除了数字还有几个个特殊的符号就是"*"、"/"和"-"、","，
       *代表所有的取值范围内的数字，
       "/"代表每的意思,"/5"表示每5个单位，
       "-"代表从某个数字到某个数字,","分开几个离散的数字。
       ```

6. 测试任务是否执行

   * 执行几次“ls”命令，看看一分钟过后文件有没有被创建

   * 如果任务执行失败了，可以通过以下命令查看任务日志

     ```
     # tail -f /var/log/cron
     ```

----

### [windows下实现mysql数据库自动定时备份](https://blog.csdn.net/amberom/article/details/80605788)



1. ```
   
   ```


*  方法一：**复制date文件夹备份**

  1. 假想环境：
     MySQL   安装位置：`C:\MySQL`
     论坛数据库名称为：`bbs`
     数据库备份目的地：`C:\db_bak\`

  2. 新建`db_bak.bat`，写入以下代码

     ```
     net stop mysql
     xcopy c:\mysql\data\bbs\*.* c:\db_bak\bbs\%date:~0,10%\ /S /I
     net start mysql
     ```

  3.  然后使用Windows的“[计划任务](https://blog.csdn.net/cdnight/article/details/53841921)”定时执行该批处理脚本即可。（例如：每天凌晨3点执行back_db.bat）

     > 备份和恢复的操作都比较简单，完整性比较高，控制备份周期比较灵活，例如，用%date:~0,10%。此方法适合有独立主机但对mysql没有管理经验的用户。缺点是占用空间比较多，备份期间mysql会短时间断开（例如：针对30M左右的数据库耗时5s左右）,针对%date:~0,10%的用法参考。

* 方法二：**mysqldump备份成sql文件**

  1. 假想环境：
     MySQL   安装位置：C:\MySQL
     论坛数据库名称为：bbs
     MySQL root   密码：123456
     数据库备份目的地：D:\db_backup\

  2. 新建`backup_db.bat`，写入以下代码

     ```
     @echo off
     set "Ymd=%date:~,4%%date:~5,2%%date:~8,2%"
     C:\MySQL\bin\mysqldump --opt -u root --password=123456 bbs > D:\db_backup\bbs_%Ymd%.sql
     @echo on
     ```

  3. 然后使用Windows的“计划任务”定时执行该脚本即可。

     说明：此方法可以不用关闭数据库，并且可以按每一天的时间来名称备份文件。

     > 通过%date:~5,2%来组合得出当前日期，组合的效果为yyyymmdd,date命令得到的日期格式默认为yyyy-mm-dd(如果不是此格式可以通过pause命令来暂停命令行窗口看通过%date:~,20%得到的当前计算机日期格式)，所以通过%date:~5,2%即可得到日期中的第五个字符开始的两个字符，例如今天为2009-02-05,通过%date:~5,2%则可以得到02。（日期的字符串的下标是从0开始的）

* 方法三：**利用WinRAR对MySQL数据库进行定时备份。**

  1. 把WinRAR安装到计算机上。

  2. 将下面的命令写入到一个文本文件里保存，然后将文本文件的扩展名修改成CMD。

     ```
     net stop mysql
     c:\progra~1\winrar\winrar a -ag -k -r -s d:\mysql.rar d:\mysql\data
     net start mysql
     ```

  3. 进入控制面版，打开计划任务，双击“添加计划任务”。在计划任务向导中找到刚才的CMD文件，接着为这个任务指定一个运行时间和运行时使用的账号密码就可以了。

     > 这种方法缺点是占用时间比较多，备份期间压缩需要时间，mysql断开比第一种方法更多的时间，但是对于文件命名很好。
