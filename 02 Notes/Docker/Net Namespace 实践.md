---
author: Breezy Frost
title: Net Namespace 实践
time: 2023-03-19 12:59:28
tags:
- network
- namespace
---
## 目标

1. 在一个 CentOs7 的服务器中，创建一个 Net Namespace，它与主机网络是完全隔离的；
2. 为这个 Net Namespace 创建虚拟网关并使其能 ping 同百度。

## 网络拓扑图

![image-20220111132707215.png](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/202303191317609.png)

## 实践步骤

### 查看当前宿主机的网络状态

```
[root@centos7 ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:6d:ad:36 brd ff:ff:ff:ff:ff:ff
    inet 192.168.95.191/24 brd 192.168.95.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::2367:c1ba:c9fb:5f37/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

### 创建  Net Namespace

```
// 创建了一个名为 breezyfrost 的网络命名空间
[root@centos7 ~]# ip netns add breezyfrost
[root@centos7 ~]# ip netns ls
breezyfrost

// 之后的命令都针对 breezyfrost 命名空间执行，使用 exit 退出
[root@centos7 ~]# ip netns exec breezyfrost bash
[root@centos7 ~]# ip addr
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
[root@centos7 ~]# exit
exit

// 退出后就查看的是宿主机中的网络情况
[root@centos7 ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:6d:ad:36 brd ff:ff:ff:ff:ff:ff
    inet 192.168.95.191/24 brd 192.168.95.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::2367:c1ba:c9fb:5f37/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
       
// 展示如何在宿主机的命名空间下执行 breezyfrost 命名空间的命令
[root@centos7 ~]# ip netns exec breezyfrost ip addr
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

// 命名空间 breezyfrost 下的路由表
[root@centos7 ~]# ip netns exec breezyfrost route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
```

### 创建虚拟网卡

```
// 在宿主机中创建网卡 veth1,类型为虚拟的以太网卡 veth，它有一个伙伴 veth1_p
[root@centos7 ~]# ip link add veth1 type veth peer name veth1_p

// 查看宿主机中网卡情况，发现多了两张网卡 veth1 和 veth1_p
[root@centos7 ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:6d:ad:36 brd ff:ff:ff:ff:ff:ff
    inet 192.168.95.191/24 brd 192.168.95.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::2367:c1ba:c9fb:5f37/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: veth1_p@veth1: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 5a:c4:60:3b:fe:f4 brd ff:ff:ff:ff:ff:ff
4: veth1@veth1_p: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether de:06:36:59:82:a2 brd ff:ff:ff:ff:ff:ff
```

### 将虚拟网卡移动到自定义的 Net Namaspace 下

```
[root@centos7 ~]# ip link set veth1 netns breezyfrost

// 虚拟网卡 veth1 已经移动到了 breezyfrost 下
[root@centos7 ~]# ip netns exec breezyfrost ip addr
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
4: veth1@if3: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether de:06:36:59:82:a2 brd ff:ff:ff:ff:ff:ff link-netnsid 0
[root@centos7 ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:6d:ad:36 brd ff:ff:ff:ff:ff:ff
    inet 192.168.95.191/24 brd 192.168.95.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::2367:c1ba:c9fb:5f37/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: veth1_p@if4: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 5a:c4:60:3b:fe:f4 brd ff:ff:ff:ff:ff:ff link-netnsid 0
```

### 配置虚拟网卡的 ip 地址

```
// 给 breezyfrost 中的 veth1 设置 ip 地址
[root@centos7 ~]# ip netns exec breezyfrost ip addr add 192.168.50.2/24 dev veth1
[root@centos7 ~]# ip netns exec breezyfrost ip addr
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
4: veth1@if3: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether de:06:36:59:82:a2 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.50.2/24 scope global veth1
       valid_lft forever preferred_lft forever

// 给宿主机中的 veth1_p 设置相同网络的 ip 地址
[root@centos7 ~]# ip addr add 192.168.50.3/24 dev veth1_p
[root@centos7 ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:6d:ad:36 brd ff:ff:ff:ff:ff:ff
    inet 192.168.95.191/24 brd 192.168.95.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::2367:c1ba:c9fb:5f37/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: veth1_p@if4: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 5a:c4:60:3b:fe:f4 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.50.3/24 scope global veth1_p
       valid_lft forever preferred_lft forever
```

### 启用网卡

```
// 启动 breezyfrost 下的网卡 veth1 和 lo
[root@centos7 ~]# ip netns exec breezyfrost ip link set veth1 up
[root@centos7 ~]# ip netns exec breezyfrost ip link set lo up
[root@centos7 ~]# ip netns exec breezyfrost ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
4: veth1@if3: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state LOWERLAYERDOWN group default qlen 1000
    link/ether de:06:36:59:82:a2 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.50.2/24 scope global veth1
       valid_lft forever preferred_lft forever

// 启动宿主机的网卡 veth1_p
[root@centos7 ~]# ip link set veth1_p up
[root@centos7 ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:6d:ad:36 brd ff:ff:ff:ff:ff:ff
    inet 192.168.95.191/24 brd 192.168.95.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::2367:c1ba:c9fb:5f37/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: veth1_p@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 5a:c4:60:3b:fe:f4 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.50.3/24 scope global veth1_p
       valid_lft forever preferred_lft forever
    inet6 fe80::58c4:60ff:fe3b:fef4/64 scope link 
       valid_lft forever preferred_lft forever
```

### 验证 breezyfrost 和 宿主机是否互通

```
// 在宿主机 ping breezyfrost 下的 ip
[root@centos7 ~]# ping 192.168.50.2 -c 4
PING 192.168.50.2 (192.168.50.2) 56(84) bytes of data.
64 bytes from 192.168.50.2: icmp_seq=1 ttl=64 time=0.027 ms
64 bytes from 192.168.50.2: icmp_seq=2 ttl=64 time=0.103 ms
64 bytes from 192.168.50.2: icmp_seq=3 ttl=64 time=0.101 ms
64 bytes from 192.168.50.2: icmp_seq=4 ttl=64 time=0.102 ms

--- 192.168.50.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3000ms
rtt min/avg/max/mdev = 0.027/0.083/0.103/0.033 ms

// 在 breezyfrost 下 ping 宿主机的 ip
[root@centos7 ~]# ip netns exec breezyfrost ping 192.168.50.3 -c 4
PING 192.168.50.3 (192.168.50.3) 56(84) bytes of data.
64 bytes from 192.168.50.3: icmp_seq=1 ttl=64 time=0.049 ms
64 bytes from 192.168.50.3: icmp_seq=2 ttl=64 time=0.058 ms
64 bytes from 192.168.50.3: icmp_seq=3 ttl=64 time=0.109 ms
64 bytes from 192.168.50.3: icmp_seq=4 ttl=64 time=0.108 ms

--- 192.168.50.3 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2999ms
rtt min/avg/max/mdev = 0.049/0.081/0.109/0.027 ms
```

### 设置路由条目

```
// 尝试在 breezyfrost 下 ping 宿主机的其他网卡 ip，发现网络不可达
[root@centos7 ~]# ip netns exec breezyfrost ping 192.168.95.191 -c 4
connect: 网络不可达

// 配置路由
[root@centos7 ~]# ip netns exec breezyfrost ip route add default via 192.168.50.3
[root@centos7 ~]# ip netns exec breezyfrost route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.50.3    0.0.0.0         UG    0      0        0 veth1
192.168.50.0    0.0.0.0         255.255.255.0   U     0      0        0 veth1

// 再次尝试发现可以 ping 通
[root@centos7 ~]# ip netns exec breezyfrost ping 192.168.95.191 -c 4
PING 192.168.95.191 (192.168.95.191) 56(84) bytes of data.
64 bytes from 192.168.95.191: icmp_seq=1 ttl=64 time=0.034 ms
64 bytes from 192.168.95.191: icmp_seq=2 ttl=64 time=0.109 ms
64 bytes from 192.168.95.191: icmp_seq=3 ttl=64 time=0.051 ms
64 bytes from 192.168.95.191: icmp_seq=4 ttl=64 time=0.068 ms

--- 192.168.95.191 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2999ms
rtt min/avg/max/mdev = 0.034/0.065/0.109/0.029 ms
```

### 添加 iptables FORWARD 规则，并启动转发功能
```
// 启动转发功能
[root@centos7 ~]# sysctl -w net.ipv4.ip_forward=1
net.ipv4.ip_forward = 1

// -s 192.168.50.0/24 表示源地址为 192.168.50.0/24
// -o ens33 表示出口网卡为 ens33，-j MASQUERADE 表示使用 MASQUERADE 技术进行地址转换。
[root@centos7 ~]# iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o ens33 -j MASQUERADE

// 这个个命令的作用是允许 veth1_p 这个虚拟网卡接口通过 ens33 这个物理网卡接口访问外部网络。
// -A FORWARD 表示将规则添加到 FORWARD 链中，-i veth1_p 表示输入接口为 veth1_p，-o ens33 表示输出接口为 ens33，-j ACCEPT 表示接受这个数据包。
[root@centos7 ~]# iptables -A FORWARD -i veth1_p -o ens33 -j ACCEPT
[root@centos7 ~]# iptables -A FORWARD -i ens33 -o veth1_p -j ACCEPT

// 至此，可以 ping 通百度 ip，但不能 ping 通百度的域名 www.baidu.com
[root@centos7 ~]# ip netns exec breezyfrost ping 110.242.68.66 -c 4
PING 110.242.68.66 (110.242.68.66) 56(84) bytes of data.
64 bytes from 110.242.68.66: icmp_seq=1 ttl=127 time=26.0 ms
64 bytes from 110.242.68.66: icmp_seq=2 ttl=127 time=26.1 ms
64 bytes from 110.242.68.66: icmp_seq=3 ttl=127 time=26.1 ms
64 bytes from 110.242.68.66: icmp_seq=4 ttl=127 time=26.0 ms

--- 110.242.68.66 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 26.010/26.069/26.142/0.206 ms

[root@centos7 ~]# ip netns exec breezyfrost ping www.baidu.com -c 4
ping: www.baidu.com: 未知的名称或服务
```