### Linux文件系统结构树

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\文件目录.png)

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\文件目录1.png)

* 根目录是整个系统最重要的一个目录，因为不但所有的目录都是由根目录衍生出来的， 同时根目录也与开机/还原/系统修复等动作有关。 由于系统开机时需要特定的开机软件、核心文件、开机所需程序、 函式库等等文件数据，若系统出现错误时，根目录也必须要包含有能够修复文件系统的程序才行。  

  ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\根目录.png)

* usr是Unix Software Resource的缩写， 也就是『Unix操作系统软件资源』所放置的目录，而不是用户的数据啦！这点要注意。 FHS建议所有软件开发者，应该将他们的数据合理的分别放置到这个目录下的次目录，而不要自行建立该软件自己独立的目录。 

  ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\usr.png)

* /var目录主要针对常态性变动的文件，包括缓存(cache)、登录档(log file)以及某些软件运作所产生的文件， 包括程序文件(lock file, run file)，或者例如MySQL数据库的文件等等。 

  ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\var.png)

---

##### 必须掌握的路径及文件

- /etc/sysconfig/network-scripts/ifcfg-eth0(第一块网卡配置文件)
- /etc/resolv.conf（DNS的配置文件，网卡配置文件优先resolv.conf）
- /etc/hosts(ip与域名（主机名）解析表)
- /etc/sysconfig/network（主机）
- /etc/fstab（开机自动挂载列表）
- /etc/rc.local（开机自启动文件，自启动命令，脚本）
- /etc/inittab （Linux开机运行级别配置文件）
- /etc/init.d(服务启动命令脚本目录)
- /etc/profile(全局环境变量)
- /etc/bashrc(别名)
- /usr/local(编译安装软件默认安装目录)
- /var/log/message（系统日志）
- /var/log/secure（系统安全日志）
- /var/spool/cron/root(定时任务，root目录)
- /proc/cpuinfo(系统cpu信息)
- /proc/meminfo（系统内存信息）
- /proc/loadavg(系统cpu负载程度)
- */proc/mounts*（系统挂载信息）

###### 讨论：cpu什么情况下算是负载很繁忙？

当cpu平均负载率大于CPU的核数的时候，我们就可以说，服务器cpu的负载已经很繁忙了。