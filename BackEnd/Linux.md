## check running port

1. `lsof -i tcp:<target port>` to find the PID
2. `kill -9 <PID>` to kill the process

## ssh connect

1. `chmod 400 <.pem>` to change the permission of the key file to private
2. `ssh -i <pem path> <hostname>@<adress>` to connect to the remote server

# 系统启动过程

## 内核的引导

当计算机打开电源后，首先是BIOS开机自检，按照BIOS中设置的启动设备（通常是硬盘）来启动。

操作系统接管硬件以后，首先读入 /boot 目录下的内核文件。 

## 运行 init

init 进程是系统所有进程的起点，你可以把它比拟成系统所有进程的老祖宗，没有这个进程，系统中任何进程都不会启动。

init 程序首先是需要读取配置文件 /etc/inittab

### 运行级别

许多程序需要开机启动。它们在Windows叫做"服务"（service），在Linux就叫做"守护进程"（daemon）。

init进程的一大任务，就是去运行这些开机启动的程序。

但是，不同的场合需要启动不同的程序，比如用作服务器时，需要启动Apache，用作桌面就不需要。

Linux允许为不同的场合，分配不同的开机启动程序，这就叫做"运行级别"（runlevel）。也就是说，启动时根据"运行级别"，确定要运行哪些程序。

Linux系统有7个运行级别(runlevel)：

- 运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
- 运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
- 运行级别2：多用户状态(没有NFS)
- 运行级别3：完全的多用户状态(有NFS)，登陆后进入控制台命令行模式
- 运行级别4：系统未使用，保留
- 运行级别5：X11控制台，登陆后进入图形GUI模式
- 运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

## 系统初始化

在init的配置文件中有这么一行：  si::sysinit:/etc/rc.d/rc.sysinit　它调用执行了/etc/rc.d/rc.sysinit，而rc.sysinit是一个bash  shell的脚本，它主要是完成一些系统初始化的工作，rc.sysinit是每一个运行级别都要首先运行的重要脚本。

它主要完成的工作有：激活交换分区，检查磁盘，加载硬件模块以及其它一些需要优先执行任务。 

## 建立终端 

rc执行完毕后，返回init。这时基本系统环境已经设置好了，各种守护进程也已经启动了。

init接下来会打开6个终端，以便用户登录系统

## 用户登录系统

 一般来说，用户的登录方式有三种： 

-  （1）命令行登录
-  （2）ssh登录
-  （3）图形界面登录

![bg2013081706](http://www.runoob.com/wp-content/uploads/2014/06/bg2013081706.png)

## Linux 关机

正确的关机流程为：sync > shutdown > reboot > halt

```bash
Shutdown –h now 立马关机

Shutdown –h 20:25 系统会在今天20:25关机

Shutdown –h +10 十分钟后关机

Shutdown –r now 系统立马重启

Shutdown –r +10 系统十分钟后重启

reboot 就是重启，等同于 shutdown –r now

halt 关闭系统，等同于shutdown –h now 和 poweroff
```

# 系统目录结构

**/bin**：
 bin是Binary的缩写, 这个目录存放着最经常使用的命令。

**/boot：**
这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。

**/dev ：**
dev是Device(设备)的缩写, 该目录下存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。

**/etc：**
这个目录用来存放所有的系统管理所需要的配置文件和子目录。

**/home**：
用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。

**/lib**：
这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库。

**/lost+found**：
这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

**/media**：
 linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。

**/mnt**：
系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。

**/opt**：
 这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

**/proc**：
这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。
这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的ping命令，使别人无法ping你的机器：

**/root**：
该目录为系统管理员，也称作超级权限者的用户主目录。

**/sbin**：
 s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。

**/selinux**：
 这个目录是Redhat/CentOS所特有的目录，Selinux是一个安全机制，类似于windows的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的。

**/srv**：
 该目录存放一些服务启动之后需要提取的数据。

**/sys**：
 这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs 。

该文件系统是内核设备树的一个直观反映。

当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。

**/tmp**：
这个目录是用来存放一些临时文件的。

**/usr**：
 这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。

**/usr/bin：**
系统用户使用的应用程序。

**/usr/sbin：**
超级用户使用的比较高级的管理程序和系统守护程序。

**/usr/src：**内核源代码默认的放置目录。

**/var**：
这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件

# 文件基本属性

使用ll或者ls –l命令来显示一个文件的属性以及文件所属的用户和组

在Linux中第一个字符代表这个文件是目录、文件或链接文件等等。

- 当为[ *d* ]则是目录
- 当为[ *-* ]则是文件；
- 若是[ *l* ]则表示为链接文档(link file)；
- 若是[ *b* ]则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；
- 若是[ *c* ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)。

接下来的字符中，以三个为一组，且均为『rwx』 的三个参数的组合。其中，[ r ]代表可读(read)、[ w ]代表可写(write)、[ x ]代表可执行(execute)。 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号[ - ]而已

![363003_1227493859FdXT](http://www.runoob.com/wp-content/uploads/2014/06/363003_1227493859FdXT.png)

### 更改文件属性

chown：更改文件属主，也可以同时更改文件属组

```shell
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
```

chmod：更改文件9个属性

-  r:4
-  w:2
-  x:1

```shell
 chmod [-R] xyz 文件或目录
 
 // example
 [root@www ~]# ls -al .bashrc
-rw-r--r--  1 root root 395 Jul  4 11:45 .bashrc
[root@www ~]# chmod 777 .bashrc
[root@www ~]# ls -al .bashrc
-rwxrwxrwx  1 root root 395 Jul  4 11:45 .bashrc

// change by setting symbols
-rw-r--r-- 1 root root 0 Nov 15 10:32 test1
# chmod u=rwx,g=rx,o=r  test1    // 修改 test1 权限
# ls -al test1
-rwxr-xr-- 1 root root 0 Nov 15 10:32 test1
#  chmod  a-x test1
# ls -al test1
-rw-r--r-- 1 root root 0 Nov 15 10:32 test1
```

此外， a 则代表 all 亦即全部的身份！那么读写的权限就可以写成r, w, x！也就是可以使用底下的方式来看：

| chmod | u g o a | +(加入) -(除去) =(设定) | r w x | 文件或目录 |
||||||
||

# 文件与目录管理

## 处理目录的常用命令

常见的处理目录的命令：

- ls: 列出目录
- cd：切换目录
- pwd：显示目前的目录
- mkdir：创建一个新的目录
- rmdir：删除一个空的目录
- cp: 复制文件或目录
- rm: 移除文件或目录

### ls (列出目录)

- -a  ：全部的文件，连同隐藏档( 开头为 . 的文件) 一起列出来(常用)
-  -d  ：仅列出目录本身，而不是列出目录内的文件数据(常用)
-  -l  ：长数据串列出，包含文件的属性与权限等等数据；(常用)

### mkdir (创建新目录)

-  -m ：配置文件的权限
-  -p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来

### cp (复制文件或目录)

- **-a：**相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)
-  **-d：**若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；
-  **-f：**为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；
-  **-i：**若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
-  **-l：**进行硬式连结(hard link)的连结档创建，而非复制文件本身；
-  **-p：**连同文件的属性一起复制过去，而非使用默认属性(备份常用)；
-  **-r：**递归持续复制，用於目录的复制行为；(常用)
-  **-s：**复制成为符号连结档 (symbolic link)，亦即『捷径』文件；
-  **-u：**若 destination 比 source 旧才升级 destination ！

### rm (移除文件或目录)

 -f  ：就是 force 的意思，忽略不存在的文件，不会出现警告信息

 -i  ：互动模式，在删除前会询问使用者是否动作

 -r  ：递归删除

### mv (移动文件与目录，或修改名称)

 -f  ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；

 -i  ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！

 -u  ：若目标文件已经存在，且 source 比较新，才会升级 (update

## 文件内容查看

- cat   由第一行开始显示文件内容
- tac   从最后一行开始显示，可以看出 tac 是 cat 的倒著写！
- nl   显示的时候，顺道输出行号！
- more 一页一页的显示文件内容
- less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
- head 只看头几行
- tail 只看尾巴几行

### cat

-  -A  ：相当於 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
-  -b  ：列出行号，仅针对非空白行做行号显示，空白行不标行号！
-  -E  ：将结尾的断行字节 $ 显示出来；
-  -n  ：列印出行号，连同空白行也会有行号，与 -b 的选项不同；
-  -T  ：将 [tab] 按键以 ^I 显示出来；
-  -v  ：列出一些看不出来的特殊字符

### nl

-  -b  ：指定行号指定的方式，主要有两种：
         -b a ：表示不论是否为空行，也同样列出行号(类似 cat -n)；
         -b t ：如果有空行，空的那一行不要列出行号(默认值)；
-  -n  ：列出行号表示的方法，主要有三种：
         -n ln ：行号在荧幕的最左方显示；
         -n rn ：行号在自己栏位的最右方显示，且不加 0 ；
         -n rz ：行号在自己栏位的最右方显示，且加 0 ；
-  -w  ：行号栏位的占用的位数。

### more

在 more 这个程序的运行过程中，你有几个按键可以按的：

- 空白键 (space)：代表向下翻一页；
- Enter         ：代表向下翻『一行』；
- /字串         ：代表在这个显示的内容当中，向下搜寻『字串』这个关键字；
- :f             ：立刻显示出档名以及目前显示的行数；
- q             ：代表立刻离开 more ，不再显示该文件内容。
- b 或 [ctrl]-b ：代表往回翻页，不过这动作只对文件有用，对管线无用。

### head 

-n  ：后面接数字，代表显示几行的意思 

# 用户和用户组管理

## 用户账号的管理 

### 添加

```shell
useradd 选项 用户名
useradd –d /usr/sam -m sam
useradd -s /bin/sh -g group –G adm,root gem
```

- -c comment 指定一段注释性描述。
- -d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
- -g 用户组 指定用户所属的用户组。
- -G 用户组，用户组 指定用户所属的附加组。
- -s Shell文件 指定用户的登录Shell。
- -u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。

### 删除

```shell
userdel 选项 用户名
userdel -r sam
```

选项是 -r，它的作用是把用户的主目录一起删除。 

### 修改

```shell
usermod 选项 用户名
```

选项同添加用户的选项

### 用户口令的管理

用户管理的一项重要内容是用户口令的管理。用户账号刚创建时没有口令，但是被系统锁定，无法使用，必须为其指定口令后才可以使用，即使是指定空口令。

```shell
passwd 选项 用户名
```

- -l 锁定口令，即禁用账号。
- -u 口令解锁。
- -d 使账号无口令。
- -f 强迫用户下次登录时修改口令。

## 系统用户组的管理

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同Linux 系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。

用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对/etc/group文件的更新。

### 增加

`groupadd 选项 用户组`

- -g GID 指定新用户组的组标识号（GID）。
- -o 一般与-g选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。

### 删除

`groupdel 用户组`

### 修改

`groupmod 选项 用户组`

- -g GID 为用户组指定新的组标识号。
- -o 与-g选项同时使用，用户组的新GID可以与系统已有用户组的GID相同。
- -n新用户组 将用户组的名字改为新名字

# 磁盘管理 

Linux磁盘管理常用三个命令为df、du和fdisk。

-  df：列出文件系统的整体磁盘使用量
-  du：检查磁盘空间使用量
-  fdisk：用于磁盘分区

## df

检查文件系统的磁盘空间占用情况。可以利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息。 

`df [-ahikHTm] [目录或文件名]`

-  -a  ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；
-  -k  ：以 KBytes 的容量显示各文件系统；
-  -m  ：以 MBytes 的容量显示各文件系统；
-  -h  ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
-  -H  ：以 M=1000K 取代 M=1024K 的进位方式；
-  -T  ：显示文件系统类型, 连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；
-  -i  ：不用硬盘容量，而以 inode 的数量来显示

## du

与df命令不同的是Linux du命令是对文件和目录磁盘使用的空间的查看

`du [-ahskm] 文件或目录名称`

- -a  ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。
-  -h  ：以人们较易读的容量格式 (G/M) 显示；
-  -s  ：列出总量而已，而不列出每个各别的目录占用容量；
-  -S  ：不包括子目录下的总计，与 -s 有点差别。
-  -k  ：以 KBytes 列出容量显示；
-  -m  ：以 MBytes 列出容量显示；

## fdisk

fdisk 是 Linux 的磁盘分区表操作工具

`fdisk [-l] 装置名称`

-l  ：输出后面接的装置所有的分区内容。若仅有 fdisk -l 时， 则系统将会把整个系统内能够搜寻到的装置的分区均列出来

### 磁盘格式化

`mkfs [-t 文件系统格式] 装置文件名`

## 磁盘检验

`fsck [-t 文件系统] [-ACay] 装置名称`

-  -t : 给定档案系统的型式，若在 /etc/fstab 中已有定义或 kernel 本身已支援的则不需加上此参数
-  -s : 依序一个一个地执行 fsck 的指令来检查
-  -A : 对/etc/fstab 中所有列出来的 分区（partition）做检查
-  -C : 显示完整的检查进度
-  -d : 打印出 e2fsck 的 debug 结果
-  -p : 同时有 -A 条件时，同时有多个 fsck 的检查一起执行
-  -R : 同时有 -A 条件时，省略 / 不检查
-  -V : 详细显示模式
-  -a : 如果检查有错则自动修复
-  -r : 如果检查有错则由使用者回答是否修复
-  -y : 选项指定检测每个文件是自动输入yes，在不确定那些是不正常的时候，可以执行 # fsck -y 全部检查修复。

## 磁盘挂载与卸除

`mount [-t 文件系统] [-L Label名] [-o 额外选项] [-n]  装置文件名  挂载点`

`umount [-fn] 装置文件名或挂载点`

-  -f ：强制卸除！可用在类似网络文件系统 (NFS) 无法读取到的情况下；
-  -n ：不升级 /etc/mtab 情况下卸除。