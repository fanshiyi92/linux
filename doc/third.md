## 综合运用
将用户 colin115 下的所有进程名以 av_ 开头的进程终止:
```
ps -u colin115 |  awk '/av_/ {print "kill -9 " $1}' | sh
```

将用户 colin115 下所有进程名中包含 HOST 的进程终止:
```
ps -fe| grep colin115|grep HOST |awk '{print $2}' | xargs kill -9;
```