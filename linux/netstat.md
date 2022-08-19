# netstat

[netstat命令详解](https://www.cnblogs.com/ftl1012/p/netstat.html)

## 统计机器中网络连接各个状态个数

```sh
netstat -an | awk '/^tcp/ {++S[$NF]}  END {for (a in S) print a,S[a]}'
```

## 监控网络客户连接数

```sh
netstat -n | grep tcp | grep 侦听端口 | wc -l
```
