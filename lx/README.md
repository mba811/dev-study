# Linux

  * [《鸟哥的Linux私房菜——基础学习篇》](http://book.douban.com/subject/4889838/)
  * [《鸟哥的Linux私房菜——服务器架设篇》](http://book.douban.com/subject/2338464/)

本系列文章分为两部分：

  * 系统学习上面两本书的笔记。
  * 实际中遇到的问题及解决方案，即本文内容。

## 实际问题

## 1. 建立网络映射

  * Mac：`Finder`->`前往`->`连接服务器`->输入`smb://IPaddress/samba`->`连接`
  * Linux：`位置`->`连接服务器`->“服务类型”选择`自定义位置`->输入`smb://IPaddress/samba`->`连接`

## 2. ssh登陆失败

以root身份远程登陆服务器，密码正确，却显示如下警告：
    
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
    Someone could be eavesdropping on you right now (man-in-the-middle attack)!
    It is also possible that a host key has just been changed.
    The fingerprint for the RSA key sent by the remote host is
    3a:17:4b:6e:62:e6:94:df:09:78:99:90:51:68:18:62.
    Please contact your system administrator.
    Add correct host key in /Users/AnyaLin/.ssh/known_hosts to get rid of this message.
    Offending RSA key in /Users/AnyaLin/.ssh/known_hosts:4
    RSA host key for 222.195.93.129 has changed and you have requested strict checking.
    Host key verification failed.

解决方法：
    
    vi ~/.ssh/known_hosts     #选中最后一条登陆记录，双击`d`删除，按“：”进入末行编辑模式，输入“x”，回车
    ssh root@222.195.93.129   #再次登陆
    The authenticity of host '222.195.93.129 (222.195.93.129)' can't be established.
    RSA key fingerprint is 3a:17:4b:6e:62:e6:94:df:09:78:99:90:51:68:18:62.
    Are you sure you want to continue connecting (yes/no)?    #输入yes
    

## 3. tar.gz 文件解压

  * 打开终端
  * 进入需要解压的xxxx.tar.gz文件所在目录
  * $ tar xvfz xxxx.tar.gz

<<<<<<< HEAD

## 4. 新建文件命令
    
    touch a.txt

## 登陆与在线求助man page

* * *

### 查看内核版本
    
    uname -r

稳定版本的偶数版，如2.6.x，适合于商业与家用环境使用；开发中版本，如2.5.x，适合开发特殊功能的环境。

### 登陆

Mac：（若以root身份登陆，将username改为root）
    
    ssh username@IPaddress

Linux：(若以root身份登陆，将username删掉）Linux默认root的提示符为#,而一般身份用户的提示符为$。
    
    su - username

### 注销Linux

注销Linux并不意味着关机，只是用户离开系统。
    
    exit

### 基础命令的操作
    
    command [-options] parameter1 parameter2 ···
    
    echo $LANG   #显示目前支持的语言
    LANG=en_US   #将语言改为英文系
    date         #显示日期与实践
    cal 10 2014  #显示日历
    bc           #计算器
    quit         #退出

### 重要的热键

`[tab]`：连按两次，具有“命令补全”和“文件补齐”的作用。 `[control]+c`：中断目前程序。`[control]+d`：键盘输入结束；直接离开文字界面（相当于``exit`）。

### 在线求助 man page
    
    man command  #command是要查询的命令名称

进入man命令后，可按`空格`往下翻页，按`q`键离开。 在man page中，可以在任何时候输入`/keyword`来查询关键字，比如/date.

### 正确的关机方法
    
    who         #查看目前有谁在线
    netstat -a  #查看网络的联机状态
    ps -aux     #查看后台执行的程序

数据同步写入磁盘：为了防止不正常关机导致的内存数据没有来得及写入磁盘，在文字界面输入
    
    sync 

惯用的关机命令：
    
    shutdown -h now     #立刻关机
    shutdown -h 20:25   #晚上8点25分关机
    shutdown -h +10     #过十分钟后关机
    shutdown -r now     #立刻重启
    shutdown -r +30 ‘The system will be reboot’    #再过30分钟关机，并显示后面的消息给所有在线用户
    shutdown -k now ‘The system will be reboot’    #仅发出警告，系统并不会真正关机

重启、关机：reboot，halt，poweroff。务必用man去查询一下。

## 文件权限与目录配置

* * *

Linux最优秀的地方之一，就在于它的多用户、多任务环境。Linux一般将文件可存取访问的身份分为3个类别，分别是owner（用户）、group（用户组）、others（其他人），且3种身份都各自有自己的read，write，execute等权限。

### 用户身份与用户组记录的文件
    
    \etc\passwd     #所有系统账号的相关信息
    \etc\shadow     #个人的密码
    \etc\group      #Linux所有的组名

### 文件权限概念

当屏幕前面出现“Permission deny”的时候，肯定是权限设置错误。
    
    ls -al          #ls:list，列出所有文件的详细信息
    ls -l           #显示文件，属性的第一个字段是文件的权限，共10位，比如-rwxr-xr--，表示owner具有rwx权限，group具有rx权限，others只具有r权限

### 改变文件属性与权限
    
    chgrp       #改变文件所属用户组，具体句法记得使用 man page 查询
    chown -R 用户账号:所在组群 文件名      #改变文件所有者
    chmod       #改变文件的权限

其中，chmod修改权限的方法有两种，分别是符号法与数字法，数字法中r，w，x的数值分别是4，2，1。

要开放“目录”（注意不是“文件”）给任何人看，应该至少同时给予r和x权限，但w权限不可随便给予。

### 文件种类与扩展名

使用`ls -l`查看到的十个字符中，第一个字符为文件的类型：

  * -: 普通文件（regular file）
  * d: 目录文件（directory）
  * l: 连接文件，类似于windows的快捷方式（link）
  * b: 块设备文件（block）
  * c: 字符设备文件（character）
  * s: 套接字（sockets）
  * p: 管道（FIFO, pipe）

### Linux目录配置标准：FHS

![1](http://painterlin.com/public/img/linuxDIRTREE.gif)

根据FHS的定义，最好将`/var`独立出来，这样至少/var死掉的时候，根目录还活着，还能够进入救援模式。

### 绝对路径与相对路径

  * cd/var/log(absolute)
  * cd../var/log(relative)
  * .: 代表当前的目录，也可以用./来表示
  * ..: 代表上一层目录，也可以用../来表示

### 例子

将install.log文件复制成为LAYtest.log，并且要给linanya这个人读写权限，可以这样做：
    
    [root@localhost ~]# cp install.log LAYtest.log     #若复制文件夹，用cp -r
    [root@localhost ~]# ls -al LAYtest.log 
    -rw-r--r-- 1 root root 62826 9月  17 15:19 LAYtest.log     #虽然完成了复制，但仍然是root的文件
    [root@localhost ~]# chown linanya LAYtest.log 
    [root@localhost ~]# ls -al LAYtest.log       
    -rw-r--r-- 1 linanya root 62826 9月  17 15:19 LAYtest.log  #文件变成linanya的
    [root@localhost ~]# 

## 文件与目录管理

* * *

### 目录的相关操作
    
    .         # 代表此层目录
    ..        # 代表上一层目录
    -         # 代表前一个工作目录
    ~         # 代表『目前使用者身份』所在的家目录
    ~account  # 代表 account 这个使用者的家目录(account是个帐号名称)

  * 在所有目录底下都会存在的两个目录，分别是`.`与`..`
  * 根目录的上一层`(..)`与根目录自己`(.)`是同一个目录

#### 几个常见的处理目录的命令

  * cd：变换目录，cd是Change Directory的缩写
  * pwd：显示目前的目录，pwd是Print Working Directory的缩写
  * mkdir：创建一个新的目录
  * rmdir：删除一个空的目录
  * mv：移动文件
    
    pwd -P                   # -P：代表显示正确的完整路径，而不是连接路径
    mkdir -m xxx             # -m：直接配置文件的权限
    mkdir -p test1/test2     # -p：直接将所需要的目录(包含上一级目录)递回创建起来！
    PATH="$PATH":/root       # 将/root路径加入PATH环境变量中

### 文件与目录管理

  * 文件与目录的检视： ls
  * 复制、删除与移动： cp, rm, mv
    
    cp -a        # 将文件的所有特性都一起复制过来
    cp -p        # 连同文件的属性一起复制过去，而非使用默认属性(备份常用)
    cp -r        # 可以复制目录，但是，文件与目录的权限可能会被改变
    rm -i        # 互动模式，在删除前会询问使用者是否动作
    rm -r        # 连目录下的东西一起删掉，并且不会询问，慎用
    mv -f        # force强制移动，如果目标文件已经存在，不会询问而直接覆盖
    mv -i        # 若目标文件 (destination) 已经存在时，就会询问是否覆盖

### 文件内容查询

  * 直接检视文件内容： cat, tac, nl （常用）
  * 可翻页检视： more, less （常用）
  * 数据撷取： head, tail
  * 非纯文字档： od
  * 修改文件时间与建置新档： touch
    
    cat [-AbEnTv] filename # 由第一行开始显示文件内容。-b列出非空白行行号；-n列出所有行号。
    tac                    # 从最后一行开始显示文件内容，tac就是cat倒着写！
    nl                     # 显示文件内容，顺便输出行号
    more                   # 一页一页地显示文件内容
    less                   # 与more类似，但可以往前翻页
    head [-n number]       # 只看文件头几行，默认是10行，number是自定义行数
    tail                   # 只看文件尾几行，文件很大的时候常用
    od                     # 以二进制方式读取文件内容

### 文件与目录的默认权限与隐藏权限

  * 文件默认权限：umask
  * 文件隐藏属性： chattr, lsattr
  * 文件特殊权限：SUID, SGID, SBIT, 权限配置
  * 观察文件类型：file
    
    umask           # 后三位数是被拿走的权限分数，比如0022，u没有被拿走权限，g和o被拿走了w权限
    umask -S        # 以符号类型来显示权限
    umask number    # 配置自己需要的权限

在默认的情况中，root的umask会拿掉比较多的属性，root的umask默认是`022`， 这是基於安全的考量啦～至於一般身份使用者，通常他们的 umask 为`002` ，亦即保留同群组的写入权力。

  * 特殊权限`s`和`t`
  * Set UID，简称SUID，当s标志在文件拥有者的x项目为SUID，对目录无效
  * Set GID，简称SGID，当s标志在群组的x项目为SGID，对目录有效
  * Sticky Bit, 简称SBIT，目前只针对目录有效，对於文件已经没有效果了

### 配置SUID,SGID,SBIT权限

在原有的权限数字前面加上需要配置的权限数字。 比如`755`->`4755` ，就意味着`-rwxr-xr-x`变为了`-rwsr-xr-x`。

  * 4 为 SUID
  * 2 为 SGID
  * 1 为 SBIT
    
    chmod 4755 filename
    chmod u=rwxs,go=x test; ls -l test      # 配置权限为-rws--x--x的模样
    chmod g+s,o+t test; ls -l test          # 配置权限为-rws--s--t，即加入SGID,SBIT权限

### 命令与文件的搜寻

  * 命令档名的搜寻：which
  * 文件档名的搜寻：whereis, locate, find

### 权限与命令的关系

#### 一、让使用者能进入某目录成为『可工作目录』的基本权限为何：

  * 可使用的命令：例如 cd 等变换工作目录的命令；
  * 目录所需权限：使用者对这个目录至少需要具有 x 的权限
  * 额外需求：如果使用者想要在这个目录内利用 ls 查阅档名，则使用者对此目录还需要 r 的权限。

#### 二、使用者在某个目录内读取一个文件的基本权限为何？

  * 可使用的命令：例如本章谈到的 cat, more, less等等
  * 目录所需权限：使用者对这个目录至少需要具有 x 权限；
  * 文件所需权限：使用者对文件至少需要具有 r 的权限才行！

#### 三、让使用者可以修改一个文件的基本权限为何？

  * 可使用的命令：例如 nano 或未来要介绍的 vi 编辑器等；
  * 目录所需权限：使用者在该文件所在的目录至少要有 x 权限；
  * 文件所需权限：使用者对该文件至少要有 r, w 权限

#### 四、让一个使用者可以创建一个文件的基本权限为何？

  * 目录所需权限：使用者在该目录要具有 w,x 的权限，重点在 w 啦！

#### 五、让使用者进入某目录并运行该目录下的某个命令之基本权限为何？

  * 目录所需权限：使用者在该目录至少要有 x 的权限；
  * 文件所需权限：使用者在该文件至少需要有 x 的权限

## 磁盘与文件系统管理

* * *

### 认识EXT2文件系统

每种操作系统能够使用的文件系统并不相同。 举例来说，windows 98以前的微软操作系统主要利用的文件系统是`FAT(或FAT16)`，windows 2000以后的版本有所谓的`NTFS`文件系统，至于Linux的正统文件系统则为`Ext2`(Linux second extended file system, ext2fs)这一个。此外，在默认的情况下，windows操作系统是不会认识Linux的Ext2的。

那么文件系统是如何运行的呢？这与操作系统的文件数据有关。较新的操作系统的文件数据除了文件实际内容外，通常含有非常多的属性，例如Linux操作系统的文件权限(rwx)与文件属性(拥有者、群组、时间参数等)。 文件系统通常会将这两部份的数据分别存放在不同的区块，权限与属性放置到`inode`中，至于实际数据则放置到`data block`区块中。 另外，还有一个超级区块(`superblock`)会记录整个文件系统的整体信息，包括inode与block的总量、使用量、剩余量等。

### 文件系统的简单操作

### 磁盘的分割、格式化、检验与挂载

  * 磁盘分区： fdisk, partprobe
  * 磁盘格式化： mkfs, mke2fs
  * 磁盘检验： fsck, badblocks
  * 磁盘挂载与卸除： mount, umount
  * 磁盘参数修订： mknod, e2label, tune2fs, hdparm

### 配置启动挂载

  * 启动挂载 /etc/fstab 及 /etc/mtab
  * 特殊装置 loop 挂载(映象档不刻录就挂载使用)

### 内存置换空间(swap)之建置

  * 使用实体分割槽建置swap
  * 使用文件建置swap
  * swap使用上的限制

### 文件系统的特殊观察与操作

  * boot sector 与 superblock 的关系
  * 磁盘空间之浪费问题
  * 利用 GNU 的 parted 进行分割行为

### 重点回顾

  * 基本上 Linux 的正统文件系统为 Ext2 ，该文件系统内的信息主要有：

    * superblock：记录此 filesystem 的整体信息，包括inode/block的总量、使用量、剩余量， 以及文件系统的格式与相关信息等；
    * inode：记录文件的属性，一个文件占用一个inode，同时记录此文件的数据所在的 block 号码；
    * block：实际记录文件的内容，若文件太大时，会占用多个 block 。
  * Ext2 文件系统的数据存取为索引式文件系统(indexed allocation)

  * 需要碎片整理的原因就是文件写入的 block 太过于离散了，此时文件读取的效能将会变的很差所致。 这个时候可以透过碎片整理将同一个文件所属的 blocks 汇整在一起。

  * Ext2文件系统主要有：boot sector, superblock, inode bitmap, block bitmap, inode table, data block 等六大部分。

  * data block 是用来放置文件内容数据地方，在 Ext2 文件系统中所支持的 block 大小有 1K, 2K 及 4K 三种而已

  * inode 记录文件的属性/权限等数据，其他重要项目为： 每个 inode 大小均固定为 128 bytes； 每个文件都仅会占用一个 inode 而已； 因此文件系统能够创建的文件数量与 inode 的数量有关；

  * 文件的 block 在记录文件的实际数据，目录的 block 则在记录该目录底下文件名与其 inode 号码的对照表；

  * 日志式文件系统 (journal) 会多出一块记录区，随时记载文件系统的主要活动，可加快系统复原时间；

  * Linux 文件系统为添加效能，会让主存储器作为大量的磁盘高速缓存；

  * 实体链接只是多了一个文件名对该 inode 号码的链接而已；

  * 符号链接就类似Windows的快捷方式功能。

  * 磁盘的使用必需要经过：分割、格式化与挂载，分别惯用的命令为：fdisk, mkfs, mount三个命令

  * 启动自动挂载可参考/etc/fstab之配置，配置完毕务必使用 mount -a 测试语法正确否；

### 参考资料

  * [鸟哥的Linux私房菜 第八章](http://vbird.dic.ksu.edu.tw/linux_basic/0230filesystem.php#harddisk)
