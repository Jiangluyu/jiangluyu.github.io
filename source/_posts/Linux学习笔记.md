---
title: Linux学习笔记
date: 2020-03-20 20:28:07
categories: Linux
tags: [Linux, 初学者]
---

> 人生中第一次面试，体验难忘，收获也良多。其中一个收获便是学校要求与企业要求的巨大的不对称性，春招秋招是拉当前最符合企业要求的人上岸的。积累了第一次经验，下次努力。
>
> 参考课程（站点-组织-课程名-授课教师）：
>
> 1. 中国大学MOOC-北京邮电大学-Linux开发环境应用-蒋砚军/高占春/周安福
> 2. Coursera-

<!--more-->

## 1. 系统状态查看

### 1.1 Linux的字符终端

- 终端Terminal

  - UNIX/Linux是**多用户系统**
    - 主机连接多台字符终端
    - 字符终端作为交互式输入输出设备

  - 终端构成
    - 键盘
    - 显示器
    - RS232串行通信接口
  - 字符终端历史
    - 英文打字机（typewriter）
    - 电传打字机（teletypewriter，简写tty）
    - **字符终端，以屏幕代替卷纸打印机（仍称作tty设备）**

- 主机与终端的链接

  - 串口卡引出多个RS232串口
  - 每个RS232接口通过电缆（$\geq3%$芯）连接一台终端
  - RS232电缆早期长度限制10米，现在可达百米

- 终端与主机的功能分工

  - 终端：主机的输入输出设备

    - 终端**通过电缆将用户的按键信息送到主机，把主机发送过来的信息在屏幕上显示**

  - 主机：程序和数据的存储及处理

    - 数据及程序存放在主机的硬盘上，程序的运行也都由主机内的CPU占用主机内存来完成

    ![image-20200320210613229](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162109.png)

- 行律（line discipline）与驱动程序

  - 驱动程序
    - 驱动不同硬件
    - 与行律模块的接口：上行和下行字符流

  - **行律的作用**
    - **一行内字符的缓冲、回显与编辑，直到按下回车**
    - **数据加工，如将\n转化为\r\n**
    - **将Ctrl+C字符转化为中止进程运行的信号（signal）**

- 主机与终端之间的通信过程

  - 运行程序

    ```c
    #include <stdio.h>
    
    int main(void)
    {
    	int n;
        
        printf("Input N: ");
        scanf("%d", &n);
        printf("N * N = %d\n", n * n);
        printf("Bye!\n");
    }
    ```

  - 终端按键五次

    - 1 7 Backspace 6 Enter

    ![image-20200320211114869](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162116.png)

>  Note: 回显退格的方式为，先用\b将光标往前移一格，再输入空格，再用\b将光标往前移一格。

- 行律功能的调整

  - 必要性：输入口令时不希望回显；不希望等到回车才将缓冲区内信息传递等

  - 调整方法

    - 程序内编程

    - 相关命令stty：

      ```bash
      stty erase ^H # ^H即对应ASCII码8，即Backspace，因为有的终端不支持Backspace按键
      stty -a # 将行律中的所有信息状态打印出来
      ```

- 终端转义序列

  - 转义字符
    
    - Esc：ASCII码`1B`（dec: 27, Oct: 033）
    
      > Note：不支持输入时可以用\033
  - 主机发往终端方向数据中的转义序列的功能
    - 控制光标位置、字符颜色、字符大小等
    - 选择终端的字符集
    - 控制终端上的打印机、刷卡器、磁条器、密码键盘
  - 举例
    - Esc[2J 功能：清除屏幕
    - Esc[8A 功能：光标上移8行
    - Esc[16;8H 功能：光标上移16行8列
    - Esc[1;31m 功能：红色字符

- **终端类型**

  - **定义一组转义序列及其对应操作**
  - 例如：ANSI，VT100，VT220等

- 仿真终端和虚拟终端

  - 仿真终端

    - PC机RS232串口，运行仿真软件模拟真正终端设备的功能，如CrossTalk，Win中的超级终端
    - 仿真内容包括实现转义码序列功能

  - 虚拟终端

    - 终端与主机之间的通信由串口线变为TCP连接，双向传递字节流
    - **主机与PC通过网络相连，Client运行telnet，Server运行telnetd**
    - **安全终端，在TCP连接上加密和压缩数据，如SecureCRT与Putty**

    ![image-20200320212757623](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162128.png)

### 1.2 用户登录和联机手册的查阅

- 普通用户和超级用户

  - root用户

    - **不受权限制约，可随意修改和删除文件**
    - 误删文件可能带来严重后果

  - 创建新用户

    由root用户创建（命令：**useradd**），用户信息存放在**/etc/passwd**文件中包括**用户名**和**用户ID**，以及**Home目录**，**登录shell**（一般为bash，也可以选其他shell，或应用程序）

- 使用Linux

  - 登录成功后出现shell提示符
    - $表示Bourne Shell系列
    - #表示当前用户为root
  - **对字母大小写敏感**

- 基本Linux命令

  - **man 查阅手册**
  - **date 日期与时间**
  - **cal 日历**
  - **bc 计算器**
  - **passwd 修改口令**

- man：查阅联机手册（manual前三个字母）

  - 手册内容

    - **命令**的说明书
    - **系统调用**的使用手册
    - C语言和其他语言的**库函数**手册
    - 系统**配置文件格式**

  - 命令

    - 分页器：q-退出，space-下一页，上下箭头-上移下移

  - 用法

    - man *name*

    - man *section name*

      章节编号：**1-命令，2-系统调用，3-库函数，5-配置文件**

    - man -k *regexp*

      列出**关键字（keyword）**与正则表达式regexp匹配的手册项目录

  - 内容

    - 基本功能和语法
    - 对于C语言的函数调用，列出头文件及链接函数库
    - 功能说明
    - SEE ALSO：有关的其他项目的名字和章节号

### 1.3 时间、计算器和口令维护

- date：读取系统日期和时间

  - 用法：

    - date

      读取系统日期和时间，如Wed Nov 7 21:09:16 CST 2018

    - 根据需要定制输出格式

      date "+%Y-%m-%d %H:%M:%S Day %j"

      2018-11-07 21:09:54 Day 311

      date "+%s"

      1541596457

      - 311指今天是今年的第311天
      - 格式控制：第一个字符必须为+号，%Y-年，%m-月，%d-日，%H-时，%M-分，%S-秒
      - %s-标坐标（从UTC1970开始），常用于计算时间间隔

    - 通过NTP协议（Network Time Protocol）校对系统时间：命令ntpdate

      ntpdate 0.pool.ntp.org（设置时间，需要root用户）

      ntpdate -q 0.pool.ntp.org（query，查询时间，普通用户即可）

- cal：打印日历（calendar前三个字母）

  - 用法
    - cal
    - cal *year*
    - cal *month year*
  - 举例
    - cal 功能：打印当前月份的日历
    - cal 2020 功能：打印2020年的日历
    - cal 10 2019 功能：打印2019年10月份的日历
    - cal 12 功能：打印公元12年的日历

- bc：计算器（basic calculator缩写）

  - 功能强大

    - 基本计算器功能
    - 支持变量a~z，函数，条件，循环等编程功能
    - 进行任意精度的计算

  - 精度

    - 缺省精度

      - **bc 缺省精度为1**
      - bc -l缺省精度为小数点后20位

    - 通过scale自行决定精度

      - scale=10000

        s(1.0) 即求小数点后10000位精度的sin(1.0)的值

- passwd：更换口令（password缩写）

  - 普通用户

    - **使用passwd命令修改自己的口令，须先验证原口令**

  - root

    - 直接修改新口令

    - **可强制修改其他用户口令，但无法读取其他用户口令**

    - 例如：passwd liu

      将liu的口令强制设置为新口令

- 口令的设置与验证

  - 口令的保存：不存明码

    ![image-20200320220231764](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162154.png)

  - 验证方法：验证生成序列+输入的口令通过哈希算法后是否与存储的哈希值相同

### 1.4 了解系统状态

- 几个了解系统状态的命令
  - who：确定谁在系统中
  - uptime：了解系统启动时间及忙碌程度
  - top：列出资源占用排名靠前的进程
  - free：了解内存使用情况
  - vmstat：了解系统负载情况

- who：确定谁在系统中

  - who：列出当前登入系统的用户

    | 用户名 | 终端设备的设备文件名 | 登录时间    |
    | ------ | -------------------- | ----------- |
    | wujian | tty00                | Jul 5 14:49 |
    | sun    | tty01                | Jul 5 11:31 |

    ![image-20200320221411992](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162159.png)

    设备文件一般存放于/dev下

  - tty：打印当前终端的设备文件名

    ![image-20200320221436881](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162203.png)

  - who am i：列出当前终端上的登录用户信息

    ![image-20200320221514313](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162309.png)

  - whoami：仅列出当前终端上的登录用户名

    ![image-20200320221451656](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162312.png)

- uptime：已开机时间

  - uptime：系统运行时间，当前登入用户数，近期（1min，5min，15min）内CPU负载（平均调度队列长度）

    ![image-20200320221349193](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162317.png)

- top：列出资源占用排名靠前的进程

  - top

    ![image-20200320221621163](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162320.png)

    - 第一行，任务队列信息，同uptime命令的执行结果

    - 第二行，Tasks进程

    - 第三行，CPU状态信息

      us（user space）- 用户空间占用CPU的百分比。

      sy（sysctl）-内核空间占用CPU的百分比。

      ni-改变过优先级的进程占用CPU的百分比

      id（idolt）-空闲CPU百分比

      wa（wait）-IO等待占用CPU的百分比

      hi（Hardware Interrupts）-硬中断占用CPU的百分比

      si（Software Interrupts）-软中断占用CPU的百分比

    - 第四行，内存状态

    - 第五行，swap交换分区信息

    - 第七行以下：各进程的状态监控

      PID-进程id

      USER-用户

      PR（priority）-进程优先级

      NI（nice）-负->正，高优先级->低优先级

      VIRT（virtual）-进程逻辑地址空间大小

      RES（Resident）-驻留内存数即占用物理内存数

      SHR（share）-与其他进程共享内存数

      %CPU-占用CPU百分比

      %MEM-占用内存百分比

      TIME+-占用的CPU时间

      COMMAND-进程名称（命令名/命令行）

- ps（process status）：查询进程状态
  - 选项
    - 空：只列出在当前终端上启动的进程（PID，TTY，TIME，COMMAND）
    
      ![image-20200321021001829](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162325.png)
    
    - -e：列出系统中所有进程
    
      ![image-20200321021027911](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162327.png)
    
    - -f：以full格式列出每一个进程
    
      ![image-20200321021047262](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162330.png)
    
    - -l：以long格式列出每一个进程
    
      ![image-20200321021058686](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162333.png)
  - 进程属性
    - UID（user id）
    - PID（process id）
    - PPID（parent process id）
    - C（CPU）：最近一段时间（秒级别）CPU占用情况
    - STIME：启动时间
    - SZ：进程逻辑内存大小
    - TTY
    - COMMAND
    - WCHAN（wait channel）：进程在内核的何处睡眠
    - TIME：累计执行时间（占用CPU的时间）
    - PRI（priority）
    - S（status）：Sleep，Run，Zombie（结束后暂时还未关闭）

- free：了解内存使用情况

  - free

    ![image-20200321020203874](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162337.png)

    内存总量1.8GB，空闲69MB

    Linux为提高效率，利用程序暂时不用的内存，缓冲读写过的磁盘信息，减少I/O时间，当前有313MB的buffer/cache

    不计buffer/cache，系统有实际可利用资源179MB

    打印了磁盘Swap区的使用情况

- vmstat：了解系统负载
  - vmstat

    ![image-20200321020608078](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162340.png)

    - procs：r-待运行的进程数，b-处在非中断睡眠状态的进程数
    - memory：空闲的内存，buffer/cache用做缓存的内存数
    - swap：磁盘/内存的交换页数量（单位：KB/s）
    - IO：块设备的I/O块数（单位：块/s）
    - system：in（interrupt）-每秒的硬件中断数，包括时钟中断，cs（context switch）-每秒的环境切换次数
    - CPU：CPU的总使用率，us（user），sy（system），id（idle），wa（wait for disk I/O）

### 1.5 复习题

1. C语言编写的应用程序，通过printf打印一个换行符\n，但在终端上执行的是回车加换行\r\n，把换行符替换为回车换行是由下面哪个软件模块完成的？

   `Linux内核中的行律模块`

2. Linux超级用户的用户名为：

   `root`

3. 哪个命令可以获得某进程占用的逻辑内存大小？

   `top`

4. 哪个命令可以了解目前系统CPU的空闲情况？

   `uptime`

5. 传统的终端与Linux主机之间传输的是字节流。（true or false)

   `true`

6. 终端转义序列的意义在于终端收到某一特定字符序列后执行一些约定好的控制功能，而不是把这些字符显示在显示器上。（true or false)

   `true`

7. 在终端按下Ctrl-C按键一般会导致一个死循环程序中止运行，这是因为按下Ctrl-C之后终端并不向Linux输送字符，而是通过RS232接口的一条特殊信号线通知Linux主机，将进程终止。（true or false)

   `false（通过行律）`

8.  Linux命令不区分字母的大小写，一般习惯用小写字母。（true or false)

   `false（Linux区分大小写）`

9. 直接执行bc命令，后面不带任何选项，除法计算时保留小数点后20个有效数字。（true or false)

   `false（默认精度为1）`

10. Linux中超级用户的权限很大，可以读取普通用户的口令值。（true or false)

    `false（只能强制修改，不能读取）`

## 2. 文本文件的处理

### 2.1 文本文件及处理工具

- Linux中的文本信息

  - 文本文件
    - C语言、Java语言等的编程文件
    - 文本格式的数据文件
    - 文本格式的文字信息
  - 程序输出
  - 系统配置信息
    - /etc下的配置文件（类似win下的注册表）
  - 文本型网络协议
    - 大部分传输层以上协议
    - 会话层协议：HTTP，POP3，SMTP，IMAP
    - 表示层协议：HTML，XML，MIME
  - 文本文件处理的命令
    - Linux提供大量 的文本文件处理的命令
    - 命令自带的选项

- 进程的标准输入/输出

  - 进程的基本概念

    - 进程和程序

      程序：存储在计算机上的代码文件

      进程：经过编译后正在运行的程序

  - 进程的输入输出
    - stdin，默认键盘
    - stdout，默认屏幕

- 重定向与管道

  - 重定向机制

    - 输出重定向

      如：ls -l  > filelist.txt

      > Note：本身ls -l会将信息显示在屏幕上，上述命令将其写入filelist.txt文件中，不会再打印在屏幕上

    - 输入重定向

      如：sort < filelist.txt

      > Note：sort默认从stdin中获取输入，上述命令使sort从filelist.txt中获取输入

    - 重定向机制与管道机制的重要性

- 文本文件处理命令的特点

  - 特点

    - 不指定对象时，默认从stdin获得数据
    - 制定对象时，从对象中获得数据
    - 多数命令可以指定多个文件
    - 处理结果将在stdout显示

  - 考虑的因素

    - 标准输入/标准输出
    - shell的文件通配符（可以同时处理多个对象）
    - 输入输出重定向
    - 管道

  - 灵活性：工具命令的组合

    - Linux倾向于提供独立的多个精巧的工具命令，数据格式为文本信息

    - 鼓励使用重定向或管道机制将多个工具命令组合，提供更灵活的功能

    - 应用系统设计时，也应该考虑到这些特点

      如数据库显示，直接输出多列文本，考虑到各种工具软件的使用

### 2.2.1 读取文件内容

- 文本文件读取与处理的几个命令
  - more/less：逐屏显示文件
  - cat（concatenate）/od（octal dump）：列出文件内容
  - head/tail：列出文件的头部/尾部
  - tee：三通
  - wc（word count）：字计数
  - sort：对文件内容排序
  - tr（translate）：翻译字符
  - uniq（unique）：筛选重复行

- more/less：逐屏显示文件

  - 历史
    - more：BSD UNIX开发
    - less：Linux广泛使用
  - 使用方法：
    - more shudu.c 指定单个文件
    - more *.[ch] 指定多个文件
    - ls -l| more 指定0个文件（因为从管道中获取输入）
    - less shudu.c

- more

  - more

    满屏后，显示--more--或--more--(15%)，more命令

    - 空格：显示下一屏
    - 回车：下移一行
    - q（quit）
    - /*pattern*：搜索指定pattern的字符串，正则表达式
    - /：继续查找指定模式的字符串
    - h（help）
    - ^L：屏幕刷新

- less

  - less：来源为less is more
    - 回退浏览功能更强：上下箭头键、J、K、PgUp等键
    - 有些Unix系统不提供less命令

- cat/od：列出文件内容

  - 命名与功能

    - cat：concatenate：串结，文本格式打印（-n：显示行号）
    - od：octal dump：逐字节打印（-c，-t c，-t x1，-t d1，-t u1）

  - 举例

    - cat tryl.c

    - cat -n shudu.c

    - cat tryl.c tryx.c try.h

    - cat > try 命令行参数为0个，直接从stdin获取数据，知道^D，将内容写入try文件中

    - cat tryl.c try2.c try.h > trysrc

    - cat makefile *.[ch] > src

    - od -t x1 x.dat 以十六进制打印文件x.dat的各字节

    - od -t x1 x.dat | more

    - od -c /bin/bash **逐字符**打印文件，遇到不可打印字符则打印编码

    - echo abcdABCD | od -t x1 十六进制显示abcdABCD的ASCII码

      ![image-20200321161435322](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321162346.png)

- head/tail：显示文件的头部和尾部

  默认10行，-n可以指定行数

  - head -n 15 ab.c

  - head -n 23 a.c b.c c.c | more

  - tail -n 40 a.txt

  - head -n -20 msg.c 除去尾部20行，其余均显示

  - tail -n +20 msg.c 除去开头20行，其余均显示

  - **tail -f debug.txt 实时打印文件尾部被追加的内容**

    > 组合运用如：netstat -s -p tcp | head, ls -s | sort | head -n 20

### 2.2.2 文本数据的处理

- tee：三通

  - 功能

    将stdin得到的数据抄送stdout，同时存入文件

  - 举例

    ./myap | tee myap.log

- wc(word count)：字计数

  - 功能

    - 列出文件的行数，单词数，字符数
    - 当文件数$\gt1$时，最后还列出一个合计
    - 常用选项-l，只列出行数

  - 举例

    - wc sum.c

    - wc x.c makefile start.sh

    - wc -l *.c makefile start.sh

    - ps -ef | wc -l

    - ps -ef | grep liang | wc -l

      > Note：grep为正则表达式或查找字符串，上述命令即在所有full格式的进程信息中查找包含liang字符串的进程数（即行数）

    - who | wc -l

- sort：对文件内容排序

  - sort选项

    - -n（numeric）：对于数字按照算术值大小排序，而非字符串比较规则排序

      > Note：字符串规则排序时，67$\gt$123

    - 可以选择行中某一部分作为排序关键字
    - 选择升序或降序
    - 字符串比较时对字母是否区分大小写
    - 内排序，外排序等算法的选择

  - 举例

    - sort telno > telno1

    - ls -s | sort | tail -n 10

    - ls -s | sort -n | tail -10

      > Note：tail -10与tail -n 10等价，即显示倒数10行

- tr（translate）：翻译字符

  - 用法

    tr *string1* *string2*

    把stdin拷贝到stdout，字符串1中出现的字符替换为字符串2中的对应字符

  - 举例

    - cat linuxlearn.txt | tr u U

      ![image-20200321165209727](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321165219.png)

    - cat linuxlearn.txt | tr '[a-z]' '[A-Z]'

      ![image-20200321165253259](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321165330.png)

    - cat file1 | tr '\012' % 将换行符换为%

      ![image-20200321165434265](https://hexo-jiangluyu.oss-cn-shanghai.aliyuncs.com/20200321165437.png)

- uniq（unique）：筛选文件中的重复行

  - 用法

    - uniq *options*
    - uniq *options inputfile*
    - uniq *options inputfile outputfile*

  - 重复的行

    **重复的行定义为紧邻两行的内容相同**

  - 选项
    - 空：打印没有重复的行与有重复的行（但仅打印一次）
    - -u（unique）：只保留没有重复的行
    - -d（duplicated）：只保留有重复的行，但仅打印一次
    - -c（count）：计数同样的行出现几次

### 2.3 复习题

1. 使用less命令逐屏显示文本文件时，使得显示内容上滚一行而不是滚动一屏，应按下哪个键？

   `回车/↓`

2. Linux中用来实现计数功能，比如：统计系统有多少个登录用户，实现计数功能的命令是：

   `wc -l`

3. Linux使用|符号连接两个命令使用管道机制，设计管道机制的目的是：

   `将前一个命令的输出作为下个命令的输入，提供更灵活的功能`

4. uniq命令可以通过它的选项，选择打印所有只出现一次的行，或者打印出现不只一次的行，或者两种都选。但无论哪种情况，重复出现的行最多只能打印一次。（true or false）

   `true`

5. 一个应用程序的C语言源程序通过printf语句在标准输出输出信息，运行时只要使用输出重定向机制，不需要修改原先的程序加入文件操作的代码，就可以把输出结果存入指定名字的文件。（true or false）

   `true`

6. less命令时more命令的一个简化版本，精简后功能比more弱，但更节约内存和CPU。（true or false）

   `false（less和more是两种实现不同的命令）`

7. tail命令的-f选项可以让tail命令持续运行下去，持续地将它操作的文本文件新增的数据显示出来。如果这个文本文件被其他进程随时间推移断断续续追加几行，tail也会断断续续地输出这些新增的内容。（true or false）

   `true`

8. 可以为tee命令提供一个文件名abc.log，例如：xyz | tee abc.log 那么，通过管道的方式可以把前面xyz命令的输出结果在当前终端上显示的同时也存入磁盘文件abc.log，可供事后查阅。如果以某用户正在使用的终端的设备文件名(如/dev/pts/2)代替文件名abc.log，那么，这个xyz命令执行时的输出就会同时在两个终端上实时显示。就算是把前面的xyz命令换成vi也是完全可能的，也就是说完全可能在第二个终端上实时看到第一个终端上的编辑画面。（true or false）

   `true`

9. 不带任何选项的uniq命令消除数据中重复的行。一旦某一行出现过，uniq会记录下来，以后无论这一行在以后什么地方再次出现，输出时都会被忽略，保证数据的唯一行。（true or false）

   `false（重复的行的定义仅为相邻的两行是否相同）`

10. 信息由一个个字节组成，tr命令处理这些信息时，可以将256种字节值中的任何一种取值“翻译”为另一个字节值，并且不限于可打印字符之间的转译，比如把换行符替换为斜线。（true or false）

    `true`