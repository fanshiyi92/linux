## 综合运用
将用户 colin115 下的所有进程名以 av_ 开头的进程终止:
```
ps -u colin115 |  awk '/av_/ {print "kill -9 " $1}' | sh
```

将用户 colin115 下所有进程名中包含 HOST 的进程终止:
```
ps -fe| grep colin115|grep HOST |awk '{print $2}' | xargs kill -9;
```

查询 7902 端口现在运行什么程序
```
#分为两步
#第一步，查询使用该端口的进程的 PID；
    $lsof -i:7902
    COMMAND   PID   USER   FD   TYPE    DEVICE SIZE NODE NAME
    WSL     30294 tuapp    4u  IPv4 447684086       TCP 10.6.50.37:tnos-dp (LISTEN)

#查到 30294
#使用 ps 工具查询进程详情：
$ps -fe | grep 30294
tdev5  30294 26160  0 Sep10 ?        01:10:50 tdesl -k 43476
root     22781 22698  0 00:54 pts/20   00:00:00 grep 1155
```
