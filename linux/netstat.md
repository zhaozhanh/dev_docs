# netstat

[netstat命令详解](https://www.cnblogs.com/ftl1012/p/netstat.html)

## 统计机器中网络连接各个状态个数

```sh
netstat -an | awk '/^tcp/ {++S[$NF]}  END {for (a in S) print a,S[a]}'
```
