
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


