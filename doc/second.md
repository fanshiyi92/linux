
## 列出目录项 ls

显示当前目录下的文件 ls
按时间排序，以列表的方式显示目录项 ls -lrt（l-长列表 r-reverse 反向 t-time）

以上这个命令用到的频率如此之高，以至于我们需要为它建立一个快捷命令方式:

在.bashrc 中设置命令别名:
```
alias lsl='ls -lrt'
alias lm='ls -al|more'
```
注：.bashrc 在当前用户目录下（可使用 cd ~ 进入），以隐藏文件的方式存储；可使用 ls -a 查看；
* 给每项文件前面增加一个 id 编号 (看上去更加整洁):
```
ls | cat -n
```

## 查找目录及文件 find/locate

搜寻文件或目录:
```
find ./ -name "core*" | xargs file
```

递归当前目录及子目录删除所有 .o 文件:
```
find ./ -name "*.o" -exec rm {} \;
```
删除 a、b 开头的文件
```
rm -f {`find ./ -name "a*" | xargs file`,`find ./ -name "b*" | xargs file`}
```
find 是实时查找，较慢；locate 会为文件系统建立索引数据库，如果有文件更新，需要定期执行更新命令来更新索引库，更快:
```
locate string
```
root 使用命令 `updatedb` 更新 locate 查找信息的系统数据库；

## 查看文件内容 cat/vi/head/tail/more

显示时同时显示行号/按页显示列表内容/查看两个文件间的差别
```
cat -n file1
ls -al | more
diff file1 file2
```

## 查找文件内容 grep

使用 egrep 查询文件内容:
```
egrep '03.1\/CO\/AE' TSF_STAT_111130.log.012
egrep 'A_LMCA777:C' TSF_STAT_111130.log.035 > co.out2
```
查找 record.log 中包含 AAA，但不包含 BBB 的记录的总数:
```
cat -v record.log | grep AAA | grep -v BBB | wc -l
```

## 文件与目录权限修改 chown/chmod

* 改变文件的拥有者 chown
* 改变文件读、写、执行等属性 chmod
* 递归子目录修改： chown -R tuxapp source/
* 增加脚本可执行权限： chmod a+x myscript
* chmod 详解
  
  Linux/Unix 的文件调用权限分为三级: 文件拥有者、群组、其他。利用 chmod 可以藉以控制文件如何被他人所调用。

  语法：
  ```
  chmod [-cfvR] [--help] [--version] mode file...
  ```
  mode : 权限设定字串，格式如下：
  ```
  [ugoa...][[+-=][rwxX]...][,...]
  ```
  >u 表示该文件的拥有者，g 表示与该文件的拥有者属于同一个群体 (group) 者，o 表示其他以外的人，a 表示这三者皆是。
\+ 表示增加权限、- 表示取消权限、= 表示唯一设定权限。
r 表示可读取，w 表示可写入，x 表示可执行，X 表示只有当该文件是个子目录或者该文件已经被设定过为可执行。

    将文件 file1.txt 设为所有人皆可读取 :
    ```
    chmod a+r file1.txt
    ```
    语法二（权限）
    ```
    chmod abc file
    ```
    其中 a,b,c 各为一个数字，分别表示 User、Group、Other 的权限。

   **r=4，w=2，x=1**

 * 另外 
  
   root 用户创建了一个上网认证程序 netlogin，如果其他用户要上网也要用到这个程序，那就需要 root 用户运行`chmod 755 netlogin`命令使其他用户也能运行 netlogin。

   但是 netlogin 执行时可能需要访问一些只有 root 用户才有权访问的文件，那么其他用户执行 netlogin 时可能因为权限不够还是不能上网。

   这种情况下，就可以用`chmod 4755 netlogin`设置其他用户在执行 netlogin 也有 root 用户的权限，从而顺利上网。

## 管道和重定向

* 批处理命令连接执行，使用 |
* 串联: 使用分号 ;
* 前面成功，则执行后面一条，否则，不执行:&&
* 前面失败，则后一条执行: ||

提示执行命令是成功还是失败：
```
ls /proc && echo  suss! || echo failed.
```
或者
```
if ls /proc; then echo suss; else echo fail; fi
```

* 重定向
  
"<"表示输入重定向

">"表示输出重定向

"<< tag"将开始标记 tag 和结束标记 tag 之间的内容作为输入

而">>"输出，表示追加到文件中，不覆盖。当前输出内容会追加到指定文件的尾部。
```
ls > list.txt 2>&l
```
```
wc < list.txt
```
```
wc << aa
>1
>aa
1 1 2
```
```
ls >> list.txt
```
**注意** 一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：

* 标准输入文件 (stdin)：stdin 的文件描述符为 0，Unix 程序默认从 stdin 读取数据。
* 标准输出文件 (stdout)：stdout 的文件描述符为 1，Unix 程序默认向 stdout 输出数据。
* 标准错误文件 (stderr)：stderr 的文件描述符为 2，Unix 程序会向 stderr 流中写入错误信息。

如果希望 stderr 重定向到 file，可以这样写：
```
command 2 > file
```
如果希望将 stdout 和 stderr 合并后重定向到 file，可以这样写：
```
command > file 2>&1

command >> file 2>&1
```

## Bash 快捷输入或删除

```
Ctl-U   删除光标到行首的所有字符,在某些设置下,删除全行
Ctl-W   删除当前光标到前边的最近一个空格之间的字符
Ctl-H   backspace,删除光标前边的字符
Ctl-R   匹配最相近的一个文件，然后输出
```

## 查看磁盘空间 df/du

查看磁盘空间利用大小:
```
df -h
```
查看当前目录所占空间大小:
```
du -sh
```
* -h 人性化显示（即带单位：比如 M/G，如果不加这个参数，显示的数字以 Byte 为单位）
* -s 递归整个目录的大小

## 打包/压缩 tar/gzip

```
tar -cvf etc.tar /etc 仅打包，不压缩！
gzip demo.txt 压缩，生成 .gz
gunzip   demo.tar.gz 解压
tar -xvf demo.tar  解压
```

## 查询进程 ps

```
ps -ef
```
显示进程信息，并实时更新
[top 详解 ](https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/top.html#top)
```
top
```
losf 详解
```
-a：列出打开文件存在的进程；
-c<进程名>：列出指定进程所打开的文件；
-g：列出 GID 号进程详情；
-d<文件号>：列出占用该文件号的进程；
+d<目录>：列出目录下被打开的文件；
+D<目录>：递归列出目录下被打开的文件；
-n<目录>：列出使用 NFS 的文件；
-i<条件>：列出符合条件的进程。（4、6、协议、:端口、@ip ）
-p<进程号>：列出指定进程号所打开的文件；
-u：列出 UID 号进程详情；
-h：显示帮助信息；
-v：显示版本信息。
```
实例：
```
lsof
command     PID USER   FD      type             DEVICE     SIZE       NODE NAME
init          1 root  cwd       DIR                8,2     4096          2 /
init          1 root  rtd       DIR                8,2     4096          2 /
init          1 root  txt       REG                8,2    43496    6121706 /sbin/init
init          1 root  mem       REG                8,2   143600    7823908 /lib64/ld-2.5.
...
```
lsof 输出各列信息的意义如下：

* COMMAND：进程的名称
* PID：进程标识符
* PPID：父进程标识符（需要指定-R 参数）
* USER：进程所有者
* PGID：进程所属组
* FD：文件描述符，应用程序通过文件描述符识别该文件。

其他例子：

查看端口占用的进程状态
```
lsof -i:3306
```
查看用户 username 的进程所打开的文件
```
lsof -u username
```
查询 init 进程当前打开的文件
```
lsof -c init
```

## 杀死相关进程 kill

## 找出系统瓶颈的利器 sar

sar 是 System Activity Reporter（系统活动情况报告）的缩写。sar 工具将对系统当前的状态进行取样，然后通过计算数据和比例来表达系统的当前运行状态。它的特点是可以连续对系统取样，获得大量的取样数据；取样数据和分析的结果都可以存入文件，所需的负载很小。sar 是目前 Linux 上最为全面的系统性能分析工具之一，可以从 14 个大方面对系统的活动进行报告，包括文件的读写情况、系统调用的使用情况、串口、CPU 效率、内存使用状况、进程活动及 IPC 有关的活动等，使用也是较为复杂。

[sar 工具使用介绍 ](https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/sar.html#sar)

## 查询网络服务和端口 netstat 

* 语法
```
netstat [-acCeFghilMnNoprstuvVwx][-A<网络类型>][--ip]
```
参数说明：
```
-a/–all 显示所有连线中的 Socket。
-A<网络类型>/–<网络类型> 列出该网络类型连线中的相关地址。
-c/–continuous 持续列出网络状态。
-C/–cache 显示路由器配置的快取信息。
-e/–extend 显示网络其他相关信息。
-F/–fib 显示 FIB。
-g/–groups 显示多重广播功能群组组员名单。
-h/–help 在线帮助。
-i/–interfaces 显示网络界面信息表单。
-l/–listening 显示监控中的服务器的 Socket。
-M/–masquerade 显示伪装的网络连线。
-n/–numeric 直接使用 IP 地址，而不通过域名服务器。
-N/–netlink/–symbolic 显示网络硬件外围设备的符号连接名称。
-o/–timers 显示计时器。
-p/–programs 显示正在使用 Socket 的程序识别码和程序名称。
-r/–route 显示 Routing Table。
-s/–statistice 显示网络工作信息统计表。
-t/–tcp 显示 TCP 传输协议的连线状况。
-u/–udp 显示 UDP 传输协议的连线状况。
-v/–verbose 显示指令执行过程。
-V/–version 显示版本信息。
-w/–raw 显示 RAW 传输协议的连线状况。
-x/–unix 此参数的效果和指定”-A unix”参数相同。
–ip/–inet 此参数的效果和指定”-A inet”参数相同。
```
例子：
```
netstat -antp | grep 6379
```

## ftp sftp lftp ssh

ssh 登陆远程服务器 host，ID 为用户名

```
ssh ID@host
```

ftp/sftp 文件传输：

```
sftp ID@host
```

sftp登陆后，可以使用下面的命令进一步操作：

```
get filename # 下载文件
put filename # 上传文件
ls # 列出 host 上当前路径的所有文件
cd # 在 host 上更改当前路径
lls # 列出本地主机上当前路径的所有文件
lcd # 在本地主机更改当前路径
```

lftp同步文件夹(类似rsync工具):


## 网络复制 scp

将本地localpath指向的文件上传到远程主机的path路径:

```
scp localpath ID@host:path
```

## 环境变量

bashrc 与 profile 都用于保存用户的环境信息，bashrc 用于交互式 non-loginshell，而 profile 用于交互式 login shell。

/etc/profile，/etc/bashrc 是系统全局环境变量设定
~/.profile，~/.bashrc 用户目录下的私有环境变量设定

当登入系统获得一个 shell 进程时，其读取环境设置脚本分为三步:

首先读入的是全局环境变量设置文件/etc/profile，然后根据其内容读取额外的文档，如/etc/profile.d 和/etc/inputrc
读取当前登录用户 Home 目录下的文件~/.bash_profile，其次读取~/.bash_login，最后读取~/.profile，这三个文档设定基本上是一样的，读取有优先关系
读取~/.bashrc
~/.profile 与~/.bashrc 的区别:

这两者都具有个性化定制功能
~/.profile 可以设定本用户专有的路径，环境变量，等，它只能登入的时候执行一次
~/.bashrc 也是某用户专有设定文档，可以设定路径，命令别名，每次 shell script 的执行都会使用它一次
例如，我们可以在这些环境变量中设置自己经常进入的文件路径，以及命令的快捷方式：

```
.bashrc
alias m='more'
alias cp='cp -i'
alias mv='mv -i'
alias ll='ls -l'
alias lsl='ls -lrt'
alias lm='ls -al|more'

log=/opt/applog/common_dir

.bash_profile
. /opt/app/tuxapp/openav/config/setenv.prod.sh.linux
export PS1='$PWD#'
```
通过上述设置，我们进入 log 目录就只需要输入`cd $log`即可；

## 系统管理

* 查看Linux系统版本
```
uname -a
lsb_release -a
```

查看Unix系统版本：操作系统版本:
```
more /etc/release
```

查看CPU信息:
```
cat /proc/cpuinfo
```

查看CPU的核的个数:
```
cat /proc/cpuinfo | grep processor | wc -l
```

查看内存信息:
```
cat /proc/meminfo
```

显示架构:
```
arch
```

设置系统日期和时间(格式为2014-09-15 17:05:00):
```
date -s 2014-09-15 17:05:00
```

```
```