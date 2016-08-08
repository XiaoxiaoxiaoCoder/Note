---
title: netstat 命令常用用法
date: 2016-08-08 19:46:36
categories: Linux
tags: Linux 命令
---

## 简介
Netstat 是一款命令行工具，可用于列出系统上所有的网络套接字连接情况，包括 tcp, udp 以及 unix 套接字，另外它还能列出处于监听状态（即等待接入请求）的套接字。如果你想确认系统上的 Web 服务有没有起来，你可以查看80端口有没有打开。以上功能使 netstat 成为网管和系统管理员的必备利器。

以下简介来至`netstat`的man手册：  
>netstat - 打印网络连接、路由表、连接的数据统计、伪装连接以及广播域成员。

<!-- more -->

## 常见参数

+ -a (all)显示所有选项，默认不显示LISTEN相关  
+ -t (tcp)仅显示tcp相关选项  
+ -u (udp)仅显示udp相关选项  
+ -n 拒绝显示别名，能显示数字的全部转化成数字  
+ -l 仅列出有在 Listen (监听) 的服務状态  
+ -p 显示建立相关链接的程序名  
+ -r 显示路由信息，路由表  
+ -e 显示扩展信息，例如uid等  
+ -s 按各个协议进行统计  
+ -c 每隔一个固定时间，执行该netstat命令  
>提示：LISTEN和LISTENING的状态只有用-a或者-l才能看到

## 常用实例

### 列出所有端口  
>最简单的命令，使用 `-a` 选项即可，即 netstat -a

```shell
[Kevin@Local200-75 ~]$ netstat -a | more
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address               Foreign Address             State
tcp        0      0 *:6528                      *:*                         LISTEN
tcp        0      0 *:8960                      *:*                         LISTEN
tcp        0      0 *:fodms                     *:*                         LISTEN
tcp        0      0 *:8512                      *:*                         LISTEN
tcp        0      0 *:afs3-update               *:*                         LISTEN
tcp        0      0 *:6304                      *:*                         LISTEN
tcp        0      0 *:dlip                      *:*                         LISTEN
tcp        0      0 *:8513                      *:*                         LISTEN
tcp        0      0 *:afs3-rmtsys               *:*                         LISTEN
tcp        0      0 *:9601                      *:*                         LISTEN
```

### 只列出 TCP 或 UDP 协议的链接
> 使用 `-t` 选项列出 TCP 协议链接

```
[Kevin@Local200-75 ~]$ netstat -at | more
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address               Foreign Address             State
tcp        0      0 *:6528                      *:*                         LISTEN
tcp        0      0 *:8960                      *:*                         LISTEN
tcp        0      0 *:fodms                     *:*                         LISTEN
tcp        0      0 *:8512                      *:*                         LISTEN
```
> 使用 `-u` 选项列出 TCP 协议链接  

```
[Kevin@Local200-75 ~]$ netstat -au | more
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address               Foreign Address             State
udp        0      0 *:syslog                    *:*
udp        0      0 *:37013                     *:*
udp        0      0 *:55200                     *:*
udp        0      0 *:snmp                      *:*
```

### 禁用别名显示，显示IP
>使用 `-n` 禁止展示别名  

```
[Kevin@Local200-75 ~]$ netstat -ant | more
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address               Foreign Address             State
tcp        0      0 0.0.0.0:6528                0.0.0.0:*                   LISTEN
tcp        0      0 0.0.0.0:8960                0.0.0.0:*                   LISTEN
tcp        0      0 0.0.0.0:7200                0.0.0.0:*                   LISTEN
tcp        0      0 0.0.0.0:8512                0.0.0.0:*                   LISTEN
tcp        0      0 0.0.0.0:7008                0.0.0.0:*                   LISTEN
```

### 显示监听中的端口
> 使用 `-l` 展示监听的端口  

```
[Kevin@Local200-75 ~]$ netstat -tnl | more
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address               Foreign Address             State
tcp        0      0 0.0.0.0:6528                0.0.0.0:*                   LISTEN
tcp        0      0 0.0.0.0:8960                0.0.0.0:*                   LISTEN
tcp        0      0 0.0.0.0:7200                0.0.0.0:*                   LISTEN
tcp        0      0 0.0.0.0:8512                0.0.0.0:*                   LISTEN
tcp        0      0 0.0.0.0:7008                0.0.0.0:*                   LISTEN
tcp        0      0 0.0.0.0:6304                0.0.0.0:*                   LISTEN
```

### 获取进程名、进程号以及用户ID
>使用 `-p` 查看进程信息，前提有进程的相关权限

```
[Kevin@localhost103 ~]$ sudo netstat -nlpt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address               Foreign Address             State       PID/Program name
tcp        0      0 0.0.0.0:10050               0.0.0.0:*                   LISTEN      8076/zabbix_agentd
tcp        0      0 0.0.0.0:5666                0.0.0.0:*                   LISTEN      2790/xinetd
tcp        0      0 127.0.0.1:199               0.0.0.0:*                   LISTEN      2723/snmpd
tcp        0      0 :::3600                     :::*                        LISTEN      22664/sshd
```

> `-ep` 可以同时查看进程名和用户名

```
[Kevin@localhost103 ~]$ sudo netstat -lept
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address               Foreign Address             State       User       Inode      PID/Program name
tcp        0      0 *:10050                     *:*                         LISTEN      zabbix     457722759  8076/zabbix_agentd
tcp        0      0 *:nrpe                      *:*                         LISTEN      root       7302       2790/xinetd
tcp        0      0 localhost.localdomain:smux  *:*                         LISTEN      root       7218       2723/snmpd
tcp        0      0 *:trap-daemon               *:*                         LISTEN      root       219592389  22664/sshd
```
注意 - 假如你将 -n 和 -e 选项一起使用，User 列的属性就是用户的 ID 号，而不是用户名。

### 打印统计数据

```
[Kevin@localhost103 ~]$ netstat -s
Ip:
    273610077 total packets received
    0 forwarded
    0 incoming packets discarded
    273610074 incoming packets delivered
    241627567 requests sent out
Icmp:
    3716128 ICMP messages received
    10 input ICMP message failed.
    ICMP input histogram:
        destination unreachable: 134059
        timeout in transit: 2009
        redirects: 113420
        echo requests: 3466591
        echo replies: 49
    3521755 ICMP messages sent
    0 ICMP messages failed
    ICMP output histogram:
    ...
```
如果想只打印出 TCP 或 UDP 协议的统计数据，只要加上对应的选项（-t 和 -u）即可，so easy。

### 打印网络接口信息
>`-i` 选项打印网络接口信息

```
[Kevin@localhost103 ~]$ netstat -i
Kernel Interface table
Iface       MTU Met    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0       1500   0 274863481      0      0      0 273575647      0      0      0 BMRU
lo        16436   0    88519      0      0      0    88519      0      0      0 LRU
```


## 总结
`netstat`大部分功能都介绍了,如果你想知道netstat的高级功能,阅读它的手册吧！！
