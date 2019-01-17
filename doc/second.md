
## 列出目录项 ls

显示当前目录下的文件 ls
按时间排序，以列表的方式显示目录项 ls -lrt （l-长列表 r-reverse反向 t-time）

以上这个命令用到的频率如此之高，以至于我们需要为它建立一个快捷命令方式:

在.bashrc 中设置命令别名:
```
alias lsl='ls -lrt'
alias lm='ls -al|more'
```
注：.bashrc 在当前用户目录下（可使用 cd ~ 进入），以隐藏文件的方式存储；可使用 ls -a 查看；
* 给每项文件前面增加一个id编号(看上去更加整洁):
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

使用egrep查询文件内容:
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