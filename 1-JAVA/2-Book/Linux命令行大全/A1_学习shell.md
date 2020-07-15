[TOC]

#### 学习shell

##### 简单命令

date——显示当前系统的时间和日期

cal——默认显示当月日历

df——查看磁盘驱动器当前的可用空间

free——显示可用内存

exit——结束终端会话

---

##### 导航文件树

pwd——查看当前工作目录

cd——改变目录

ls——列出目录内容

---

绝对路径名：**从根目录开始**，其后紧跟一个又一个文件树分支，直到到达目标目录或文件

`cd /usr/bin`

相对路径名：**从工作目录开始**，使用“.”和“..”来表示文件系统树中的相对位置

“.”代表工作目录

“..”代表工作目录的父目录

---

快捷方式

cd：将工作目录变成主目录

cd-：将工作目录改变成先前的工作目录

cd~username：将工作目录改变为username的主目录

---

ls——列出目录的内容

* -a(--all) ：列出所有文件，包括以点号开头的文件，这些文件通常是不列出来的（隐藏文件）
* -d(--directory)：通常，如果指定了一个目录，ls命令会列出目录中的内容而不是目录本身，将此选项与-l选项结合使用，可查出目录的详细信息，而不是目录中的内容
* -F(--classify)：选项会在每个所列出的名字后面加上类型指示符（例如，名字是目录名，则会加上一个斜杠）
* -h(--human-readable)：以长格式列出，以人们可读的方式而不是字节数来显示文件大小、
* -l：使用长格式显示结果
* -r(--reverse)：以相反的顺序显示结果。通常，ls命令按照字母升序排列显示结果
* -S：按文件大小对结果排序
* -t：按修改时间排序

file filename——查看文件类型

less——查看文件文本

* PAGE UP或b：后翻页
* PAGE DOWN或spacebar：前翻页
* 上下箭头：向上下一行
* G：跳转到文本文件的末尾
* g：跳转到文本文件的开头
* /charecters：向前查找指定字符串
* n：向前查找下一个出现的字符串，这个字符串是之前所指定查找的
* h：显示帮助屏幕
* q：退出less

> 使用less命令可以分页显示任意命令的输入

----

##### 操作文件和目录

mkdir——创建目录

`mkdir directory...`

cp——复制文件和目录

`cp directory...`

* -a(--archive)：复制文件和目录及其属性，包括所有权和权限。通常来说，复制的文件具有用户操作文件的默认属性
* -i(--interactive)：在覆盖一个已存在的文件前，提示用户进行确认，如果没有指定该选项，cp会默认覆盖文件
* -r(--recursive)：递归地复制目录及其内容。复制目录时需要这个选项（或-a选项）、
* -u(--update)：当将文件从一个目录复制到另一个目录时，只会复制那些目标中不存在的文件或是目标目录相应文件的更新文件
* -v(--verbose)：复制文件时，显示信息性消息（information message）

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\cp.png)

mv——移动或重命名文件和目录

`mv item... directory`

* mv命令很多选项与cp命令是共享的

  ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\mv.png)

**rm**——移除文件和目录

`rm item...`

> 类unix系统不包括还原删除的命令

> 当rm命令与通配符一起使用四，除仔细检查输入内容外，可使用ls命令预先对通配符做出测试，这将显示欲删除的文件。紧接着，你可以按下向上方向键调用之前的命令，并使用rm代替ls

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\rm.png)

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\rm1.png)

ln——创建硬链接和符号链接

* 符号链接（允许文件有多个名字）：同一程序通过里链接（foo->foo2.6）指向不同版本(可同时存在多个版本)（快捷方式）

  * `ln -s item link`

* 硬链接：默认情况下，每个文件有一个硬链接，该链接会给文件起名字。当创建一个硬链接的时候，也为这个文件创建了一个额外的目录条目。

  * `ln file link`

  * 局限性：

    * 硬链接不能引用自身文件系统之外的文件。也就是说，链接不能引用与该链接不在同一磁盘分区的文件
    * 硬链接无法引用目录

  * 硬链接和文件本身没有什么区别。与包含符号链接的目标列表不同，包含硬链接的目录列表没有特别的链接指示说明。当硬链接删除时，只是删除可这个链接，但是文件本身的内容仍然存在（也就是说，空间没有释放），除非该文件的所有链接都被删除了

  * 创建硬链接是=时，实际上是创建了额外的名称，这些名称都指向同一数据部分

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\硬链接.png)

    第一个字段就是索引节点号，同一个索引节点号，就可以证实它们是相同的文件

----

##### 命令的使用

命令的四种情况

- 可执行程序
- shell内置命令
- shell函数
- alias命令

type——shell内置命令，可根据指定的命令名显示shell将要执行的命令类型

* `type command`

which——显示可执行程序的位置

* `which command`
* 只适用于可执行程序，而不适用于内置命令和命令别名

help——获得shell内置命令的帮助文档

* `help command`
* 出现在命令语法描述中的方括号表示一个可选的选项。竖线符号代表的是两个互斥的选项。比如cd命令：cd\[-L\|-P][dir]
* `command --help` ：显示命令的使用信息

man——显示命令的手册页

* `man program`

* `man section search_term`

* 大多数供命令行使用的可执行文件，提供一个称之为manual或者是man page的正式文档。该文档可以用一种称为man的特殊分页程序来查看

  ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\man.png)

apropos——显示一系列合适的命令

info——显示命令的info条目

​	![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\info.png)

whatis——显示一条命令的简述

alias——创建一条命令的别名

* `alias name='string'`

* 通过使用分号来分隔多条命令，就可以将多条命令输入在一行中
  * `cd /usr;ls;cd -`
* `type command`判断名称是否已被使用
* `alias foo='cd /usr; ls; cd-'`创建新命令的别名
* `unalias name`删除别名
* `alias`查看在环境中定义的所有别名

README和其他程序文档文件

* 系统中安装的很多软件包都有自己的文档文件，它们存放在`/usr/share/doc`目录中。
* gzip包包含一个特殊的less版本。称之为zless。zless可以显示由gzip压缩的文本文件的内容

-----

##### 重定向——I/O

这个功能可以把命令行的输入重定向为从文件中获取内容，也可以把命令行的输出结果重定向到文件中。

如果我们将多个命令行关联起来，将形成非常强大的命令——管道

* cat——合并文件
* sort——对文本进行排序
* uniq——报告或删除文件中重复的行
* wc——打印文件中的换行符、字和字节的个数
* grep——打印匹配行
* head——输出文件的第一部分内容
* tail——输出文件的最后一部分内容
* tee——读取标准输入的数据，并将其内容输出到标准输出和文件中

###### 标准输出（standard output）stdout

* 重定向——操作符“>”
* `ls -l /usr/bin > ls-output.txt`
* 将ls命令的结果保存到 ls-output.txt文件中 而不是显示在屏幕上
* 使用重定向“>”来重定向标准输出时，目的文件通常会从文件开头部分重新改写；若出现错误信息，则在出现错误信息的情况下停止操做
* 可以使用`> ls-output.txt`删除一个文件内容（或者创建一个新的空文件）
* 从文件尾部开始添加输出内容 使用重定向符 “>>”
* 如果这个文件不存在，将与操作符 “>” 的作用一样创建这个文件

> 文件描述符——一个程序可以把生成的输出内容发送到任意文件流中。如果把这些文件流中的前三个分别对应标准输入文件（0）、标准输出文件（1）和标准错误文件（2）。那么shell将在内部用文件描述符分别索引它们为0,1,2。

###### 标准错误（standard error）stderr

* `ls -l /bin/usr 2> ls-error.txt`

* 文件描述符“2”紧放在重定向符之前，将标准错误重定向到`ls-error.txt`

* 需要将一个命令的所有输出内容放在一个独立的文件中（包含标准输出和标准错误）

  * 传统方法 在旧shell中使用

    * `ls -l /bin/usr > ls-output.txt 2>&1`
    * 执行两个重定向操作
      * 重定向标准输出到ls-output.txt
      * 使用标记符 2>&1把文件描述符2(标准错误)重定向到文件描述符1(标准输出)中

    > 注意重定向顺序 标准错误重定向通常发生在标准输出重定向之后

  * 新版本方法

    * `ls -l /bin/usr &> ls-output.txt`

* 处理不想要的输出

  * 系统提供了一种方法，即通过把输出重定向到一个称为`/dev/null`的特殊文件中来实现
  * 这个文件是一个称为位桶（bit bucket）的系统设备，它接受输入但是不对输入进行任何处理
  * `ls -l /bin/usr 2> /dev/null`

###### 标准输入（standard input）stdin

通常来说，输出内容显示在屏幕上，输入内容来自于键盘。但是使用I/O重定向功能可以改变这一惯例

* cat——合并文件

  * `cat [file...]`

  * `cat ls-output.txt`将显示ls-output.txt文件的内容。cat经常用来显示短的文本文件。

  * cat可以接受多个文件作为参数，所以它也可以用来把文件连接在一起。假设我们下载了一个很大的文件，它已被拆分为多个部分（Usenet上的多媒体文件经常采用拆分这种方式），现在我们想要把各部分连接在一起，并还原为原来的文件。

  * 如果这些文件命名为 movie.mpeg.001 movie.mpeg.002 movie.mpeg.003 ...

  * 可以使用 `cat movie.mepg.0* > movie.mpeg`

    >  通配符一般都是按照顺序来扩展的，因此这些参数将按正确的顺序来排列··

  * `cat > lazy.txt`  输入完成后按下`Ctrl-D`简单编辑一个文本

* 重定向

  * `car < lazy.txt`
  * 使用重定向符 “<” , 将标准输入的源从键盘变为lazy.txt文件。（默认键盘输入） 

###### 管道

* 命令从标准输入到读取数据，并将数据发送到标准输出的能力，是使用了名为管道的shell特性。使用管道操作符“|” 可以把一个命令的标准输出传送到另一个命令的标准输入中。
  * `Command1|Command2`
  * `ls -l /usr/bin|less`
    * 通过使用该技术，可以很方便地检查任意一条生成标准输出的命令的运行结果

###### 过滤器

* 管道功能经常用来对数据执行复杂的操作。也可以把多条命令合在一起构成一个管道。这种方式中用到的命令通常被称为过滤器（filter）。过滤器接受输入，按照某种方式对输入进行改变，然后再输出它。

  * **sort** ——对文本进行排序

    * `ls /bin /usr/bin|sort|less`
    * 将/bin 和 /usr/bin 目录下的所有可执行程序合并成一个列表，并且按照顺序排序，最后查看这个列表

  * **uniq**——报告或者忽略文件中的重复行

    * 默认情况下，该命令删除列表中的所有重复行。因此，在管道中添加uniq命令，可以确保所有的列表都没有重复行
    * `ls /bin /usr/bin|sort|uniq|less`
    * 如果反过来想要查看重复行的列表，可以在uniq命令后面添加-d选项
    * `ls /bin /usr/bin|sort|uniq -d|less`

  * **wc**（字数统计，word count）——打印行数、字数和字节数

    * 显示文件中包含的行数、字数和字节数
    * `wc ls-output.txt`
    * -l 选项限制命令只报告行数，把它添加在管道中可以很方便地实现计数功能。如果我们要查看已好序的列表中的条目数，可以按以下方式输入
    * `ls /bin /usr/bin |sort|uniq|wc -l`

  * **grep**——打印匹配行

    * 它用来在文件中查找匹配文本
    * `grep pattern [file...]`
    * 当grep在文件中遇到“模式”的时候，将打印出包含该模式的行。
    * 列出的程序中搜索出文件名中包含zip的所有文件，该搜索将获悉系统中与文件压缩相关的程序
    * `ls /bin /usr/bin | sort | uniq |grep zip`
    * -i —— 使grep在搜索时忽略大小写（默认区分大小写）
    * -v —— 时grep只输出和模式不匹配的行

  * **head**/**tail** —— 打印文件的开头部分/结尾部分

    * 默认情况下着两个命令都是输出文件的10行内容，不过可以使用 -n 来调整输出的行数

    * `head -n 5 ls-output.txt`

    * 运用在管道中——`ls /usr/bin| tail -n 5`

    * tail 中可以使用 -f 选项来实时查看文件，该选项在观察正在被写入的日志文件的进展状态时很有用

    * `tail -f /var/log/message`

    * tail 将持续监视这个文件，一旦添加了新行，新行将会立即显示在屏幕上。按下Ctrl C后停止

      > var/log/message 文件可能包含安全信息，所有一些Linux发行版中，需要超级用户的权限才能执行该操作

  * **tee** —— 从stdin读取数据，并同时输出到stdout和文件

    * tee程序读取标准输入，再把读到的内容复制到标准输出（允许数据可以继续向下传递到管道中）和一个或更多的文件中去。
    * 当在某个中间处理阶段来捕获一个管道中的内容时，会很有用
    * `ls /usr/bin |tee ls.txt|grep zip`

##### 透过shell看世界

echo——把文本参数内容打印到标准输出

* `echo this is a test`
* `echo *`

* 在按下Enter键的时候，shell会在执行命令前自动扩展命令行中所有符合条件的字符，因此 echo 命令将不可能看到 “*” 字符，只能看到 “\*” 字符扩展后的结果。

###### 路径名扩展

* 使用通配符来实现扩展的机制称为路径名扩展（pathname expansion）
* `ls`——`echo D*`——`echo *s`——`echo [[:upper:]]`
* 查看除主目录之外的目录 `echo /usr/*/share`

> 隐藏文件的路径名扩展——`ls -d .[1.]?*`——这种模式将扩展为以一个点字符开头的所有文件名，文件名中并不包含第二个点字符，但包含至少一个额外的字符，后面也可能还跟着其他的字符

###### 波浪线扩展

* `echo ~`——`echo ~foo`
* 波浪线字符具有特殊的意义，如果把它用在一个单词的开头，那么它将被扩展为指定用户的主目录；如果没有指定用户命名，则扩展为当前用户的主目录

###### 算术扩展

* shell支持通过扩展来运行算术表达式，这允许我们把shell提示符当做计算器来使用

* `$((expression))` —— `echo $((2+2))`

* expression 是指包含数值和算术操作符的算术表达式；算术扩展只支持整数，但是可以执行很多不同的运算

  ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\算术运算符.png)

  > 空格在算术表达式中是没有意义的，而且表达式是可以嵌套的——`echo $(($((5**2))*3))`

###### 花括号扩展

* 可以按照花括号里面的模式创建多种文本字符串

  ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\花括号扩展.png)

* 用于花括号扩展的模式信息可以包含一个称为前导字符（preamble）的开头部分和一个称为附言（postscript）的结尾部分。花括号表达式本身可以包含一系列逗号分隔的字符串，也可以包含一系列整数或者单个字符。这里的模式信息不能包含内嵌的空白。

  ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\花括号扩展-1.png)

* 主要用于创建一系列的文件或者目录。

* `mkdir {2009..2011}-0{1..9} {2009..2011}-{10..12}`

###### 参数扩展

* 相当于变量名
* echo $USER
* 参数扩展若变量名拼写错误，仍然会进行扩展，只不过结果是输出一个空字符串

###### 命令替换

* 命令替换可以把一个命令的输出作为一个扩展模式使用
* `echo $(ls)`
* `ls -l $(which cp)`
  * 把`which cp`命令的运行结果作为 ls 命令的一个参数，因此我们无需知道 cp 程序所在的完整路径就能获得cp程序对应的列表。这个功能并不只是局限于简单的命令，也可以应用于整个管道中（只不过只显示部分输出内容）
* `file $(ls /usr/bin/*|grep zip)`

###### 引用

* shell提供了一种称为引用（quoting）的机制，用来有选择性地避免不想要的扩展
* 双引号
  * 如果吧文本放在双引号中，那么shell使用的所有特殊字符都将失去它们的特殊含义，而被看作普通字符
  * 字符“$”美元符号、“\” 反斜杠、“ ‘ ”反引号 除外
  * 参数扩展、算术扩展、路径名扩展和命令替换仍然生效
  * 使用双引号可以阻止单词分割甚至可以修复破损的文件名
    * `ls -l "two words.txt"`
    * `mv two "two words.txt" two_words.txt`
* 单引号
  * 抑制所有的扩展
* 转义字符
  * 经常在双引号中用来有选择性地阻止扩展
  * `echo "this is \$5.00"`
  * 用来消除文件名中某个字符的特殊含义（“$”,"!","&"空格等）
  * `mv bad\&filename good_filename`
  * 如果想要反斜杠字符 使用 “\\\\”来实现
  * 单引号中的反斜杠将失去它的特殊含义

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\转义字符.png)

##### 高级键盘命令

* clear——清屏
* history——显示历史列表记录

###### 光标移动

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\光标移动.png)

###### 修改文本

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\修改文本.png)

###### 剪切和粘贴（killing and Yanking）文本

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\剪切和粘贴.png)

* Readline文档中使用术语killing和yanking来指代通常所说的剪切和粘贴。被剪切的内容存放在一个称为kill-ring的缓冲区中

###### 自动补全功能

* Tab

###### 搜索历史命令

* `history | less`
* `history | grep /usr/bin`
  * 列出/usr/bin目录下内容的命令
* 可以通过使用历史记录扩展的扩展类型立即使用命令
  * `!12` 
* bash支持以递增方式搜索历史记录。也就是说，当搜索历史记录时，锁着输入字符数的增加，bash会相应地改变搜索范围。按下Ctrl+R键，接着输入你要查找的内容，可以开始递增式的搜索。当找到要查找的内容时，按Enter键表示执行此命令，而按Ctrl+J将把搜索到的内容从历史记录列表中复制到当前命令行。当要查找下一个匹配项时（即向前搜索历史记录），再次按下Ctrl+R键。若要退出搜索，按下Ctrl+G或者Ctrl+C即可

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\历史记录.png)

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\历史记录-1.png)

###### 历史记录扩展

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\历史记录扩展命令.png)

----

##### 脚本(script)

`script [file]`

* 其中file为用来保存会话记录的文件名。如果没有指定文件，默认使用文件typescript。脚本的man页面给出了该程序的所有可选项和特性

##### 权限

​	在unix安全模型中，一个用户可以拥有文件和目录。当一个用户拥有一个文件或者目录时，它将对该文件或目录的访问权限拥有控制权。

​	反过来，用户又归属于一个群组，该群组由一个或者多个用户组成，组中用户对文件和目录的访问权限由其所有者所有者赋予。

​	除了可以授予群组访问权限之外，文件所有者也可以授予所有用户一些访问权限.

* id——显示用户身份标识

​        用户账户定义在文件`etc/passwd`中，用户组定义在文件`etc/group`文件中。在创建用户账户和群组时，这些文件随着文件`/etc/shadow`的变动而修改，文件`/etc/passwd`中保存了用户的密码信息。

​	对于每一个用户账户，文件`/etc/passwd`中都定义了对应用户的用户名、uid、gid、账户的真实姓名、主目录以及登录shell信息。

###### 读取、写入和执行

​	对文件和目录的访问权限是按照读访问、写访问以及执行访问来定义的

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\文件属性.png)

1. 文件类型

   ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\文件类型.png)

   文件属性中剩下的9个字符称为文件模式（file mode），分别表示文件所有者、文件所属群组以及其他所有用户对该文件的读取、写入和执行权限

2. 权限属性

   ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\权限属性.png)

   ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\权限属性-1.png)

   权限属性实例

   ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\权限属性实例.png)

* chmod——更改文件的模式（权限）

  * 只有文件所有者和超级用户才可以更改文件或者目录的模式

  * 八进制数字表示法：使用八进制数字来设置所期望的权限模式

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\八进制数字表示法.png)

  * 符号表示法：chmod命令支持一种符号表示法来指定文件模式，符号分为三部分

    * 更改会影响谁
      * u：user的简写，表示文件或者目录的所有者
      * g：文件所属群组
      * o：others的简写，表示其他所有用户
      * a：all的简写，是‘u’，‘g’，‘o’三者的组合
    * 要执行哪个操作
      * 操作符‘+’表示添加一种权限，‘-’表示删除一种权限，‘=’表示只有指定的权限可用，其他所有的权限都被删除。
    * 要设置那种权限
      * 权限由字符“r”，“w”，“x”来指定

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\符号表示法.png)

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\符号表示法-1.png)

* umask——设置文件的默认权限

  * umask 命令控制着创建文件时指定给文件的默认权限。它使用八进制表示法来表示从文件模式属性中删除一个位掩码
  * `umask 0000` 关闭该功能

* su——以另一个用户的身份运行shell

  * 在shell会话状态下，使用su命令将允许你假定为另一个用户的身份，既可以以这个用户的ID来启动一个新的shell会话，也可以以这个用户的身份来发布一个命令
  * su [-[l]] \[user]
  * exit : 返回到之前的shell环境
  * su -c 'command' ：单个命令行将被传递到有个新的shell环境下进行执行。这里需要用单引号把命令行引起来。

* sudo——以另一个用户的身份来执行命令

  * 使用sudo命令将允许管理者创建一个称为`/etc/sudoer`的配置文件，并且定义一些特定的命令，这些命令只有被赋予为假定身份的特定用户才允许执行。
  * `sudo -l`查看sudo命令可以授予哪些权限

* chown——更改文件所有者和所属群组

  * 使用这个命令需要超级用户的权限

  * chown [ower]\[:[group]] file...

  * chown 命令更改的是文件所有者还是文件所属群组，或者两者都改，取决于改命令的第一个参数

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\chown命令参数.png)

* chgrp——更改文件所属群组

  * 该命令除了限制多一点之外，和chown 命令的使用方式几乎相同

* passwd——更改用户密码

  * `passwd [user]`
  * 如果更改的是用户自己的密码，那么只需要输入passwd命令。

##### 进程

​	系统启动时，内核先把它的一些程序初始化为进程，然后运行一个称为init的程序。init程序将依次运行一系列称为脚本初始化（init script）的shell脚本（放在/etc目录下），这些脚本将会启动所有的系统服务。其中很多服务都是通过守护程序（daemon program）来实现的。而后台程序只是待在后台做它们自己的事情，并且没有用户界面。因此，即使没有用户登录，系统也忙于执行一些例行程序。

​	一个程序的运行可以触发其他程序的运行，在进程系统中这种情况被表述为父进程创建子进程

​	内核会保护每个进程的信息以确保任务有序进行。比如，每个进程将被分配一个称为进程ID（PID，processID）的号码。进程ID是按递增的顺序来分配的，init进程的PID始终为1.内核也记录分配给每个进程的内存信息以及用来恢复运行的进程就绪信息。和文件系统类似，进程系统中也存在所有者、用户ID、有效用户ID等。

* ps——显示当前所有进程的运行情况

  * 默认情况下，ps命令输出的信息并不是很多，只是输出和当前会话相关的进程信息

  * TTY是teletype(电传打字机)的缩写，代表进程的控制终端（Controllering terminal）。

    * TTY列中出现“?” 表示没有终端

  * TIME字段表示了进程消耗的CPU时间总和

  * `ps x`显示所有进程

  * STAT 列是state的缩写，显示的是进程的当前状态

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\进程状态.png)

  * `ps aux` 显示属于每个用户的进程信息，使用这些选项时不带前置连字符将使得命令以“BSD模式（BSD-style）”运行。

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\BSD模式下ps命令输出的列标题.png)

* top——实时显示当前所有任务的资源占用情况

  * ps 命令提供的是ps命令被执行时刻机器状态的一个快照

  * top命令可以查看机器运行情况的动态视图

  * top命令将按照进程活动的顺序，以列表的形式持续更新显示系统进程的当前信息（默认3秒更新一次）。它主要用于查看系统“最高（top）”进程的运行情况。

  * top命令显示的内容包含两个部分，顶部显示的是系统总体状态信息，下面是一张按CPU活动时间排序的进程情况表

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\top信息中的字段.png)

* jobs——列出所有活动作业的状态信息

  * shell的作业控制特性也提供了一种方式来查看从该终端启动的所有作业。使用jobs命令可以得到如下列表信息

* bg——设置在后台中运行作业

  * 要想在启动程序时让该程序在后台运行，可以在命令后面加上 “&” 来实现
  * `program &`

* fg——设置在前台中运行作业

  * 通过fg命令后加上百分比符号和作业编号（称为jobspec选项）来实现这个功能，如果后台只有一个任务，那么可以不带jobspec选项。

  > Ctrl-Z用来暂停前台进程

* kill——发送信号给某个进程

  * `kill [-signal] PID...`

  * 如果命令行中没有指定信号，那么默认发送TERM(终止，Terminate)信号。

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\kill常用信号.png)

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\kill常用信号-1.png)

    ![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\kill其他常用信号.png)

  * `kill -l` 显示完整的信号列表

* killall——杀死指定名字的进程

  * `killall [-u user] [-signal] name...`

    > 同一程序可以启用多个进程

  * 和kill命令一样，你必须具有超级用户权限，才能够使用killall命令给不属于自己的进程发送信息

* shutdown——关机或者重启系统

* 其他进程相关命令

![](C:\Users\Maktub\Documents\Notes\0-picture\Linux\进程相关命令.png)

-----