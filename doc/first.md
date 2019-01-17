# 命令 
whatis info man which whereis 

xargs exec

* 在只记得部分命令关键字的场合，我们可通过 man -k 来搜索；
* 需要知道某个命令的简要说明，可以使用 whatis；而更详细的介绍，则可用 info 命令；whatis 等同于 man -f；
* 查看命令在哪个位置，我们需要使用 which；
* 而对于命令的具体参数及使用方法，我们需要用到强大的 man；
* xargs 命令 是给其他命令传递参数的一个过滤器，也是组合多个命令的一个工具。它擅长将标准输入数据转换成命令行参数，xargs 能够处理管道或者 stdin（标准输入，一般指键盘输入的缓存）并将其转换成特定命令的命令参数。xargs 也可以将单行或多行文本输入转换为其他格式，例如多行变单行，单行变多行。xargs 的默认命令是 echo，空格是默认定界符。这意味着通过管道传递给 xargs 的输入将会包含换行和空白，不过通过 xargs 的处理，换行和空白将被空格取代。xargs 是构建单行命令的重要组件之一。
* exec 命令 用于调用并执行指令的命令。exec 命令通常用在 shell 脚本程序中，可以调用其他的命令。如果在当前终端中使用命令，则当指定的命令执行完毕后会立即退出终端。
## man 详解：

* 语法
> man (选项) (参数)

* 选项
>-a：在所有的 man 帮助手册中搜索；
-f：等价于 whatis 指令，显示给定关键字的简短描述信息；
-P：指定内容时使用分页程序；
-M：指定 man 手册搜索的路径。

* 参数
> 数字：指定从哪本 man 手册中搜索帮助；
关键字：指定要搜索帮助的关键字。

* 实例
> 我们输入
```shell
man ls
```
> 它会在最左上角显示“LS（1）”，在这里，“LS”表示手册名称，而“（1）”表示该手册位于第一节章，同样，我们输 man ifconfig 它会在最左上角显示“IFCONFIG（8）”。也可以这样输入命令：“man [章节号 ] 手册名称”。

>man 是按照手册的章节号的顺序进行搜索的，比如：
>
```shell
man sleep
```
> 只会显示 sleep 命令的手册，如果想查看库函数 sleep，就要输入

```shell
man 3 sleep
```

## xargs 详解
xargs用作替换工具，读取输入数据重新格式化后输出。

定义一个测试文件，内有多行文本数据：
```
cat test.txt

a b c d e f g
h i j k l m n
o p q
r s t
u v w x y z
```
多行输入单行输出：
```
cat test.txt | xargs

a b c d e f g h i j k l m n o p q r s t u v w x y z
```
**-n** 选项 多行输出：
```
cat test.txt | xargs -n3

a b c
d e f
g h i
...
```
**-d** 选项 可以自定义一个定界符：
```
echo "nameXnameXnameXname" | xargs -dX

name name name name
```

* 读取 stdin，将格式化后的参数传递给命令

xargs 的一个选项 **-I** ，使用 -I 指定一个替换字符串 {}，这个字符串在 xargs 扩展时会被替换掉，当 -I 与 xargs 结合使用，每一个参数命令都会被执行一次：
```
cat arg.txt | xargs -I {} ./sk.sh -p {} -l

-p aaa -l
-p bbb -l
-p ccc -l
```
（详情参考资料）

* xargs 其他应用

假如你有一个文件包含了很多你希望下载的 URL，你能够使用 xargs 下载所有链接
```
cat url-list.txt | xargs wget -c
```