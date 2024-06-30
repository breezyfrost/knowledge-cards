---
author: Breezy Frost
title: Linux Namespace 初识
time: 2023-03-19 10:09:12
tags:
- docker
- linux
---
## 什么是 Namespace

![image.png](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/202303191049631.png)

> Namespace 是 Linux kernel 的一项特性，它对内核资源进行分区，以便一组进程看到一组资源，而另一组进程则看到不同的资源。该功能的工作原理是为一组资源和进程分配一组相同的 namespace，但这些 namespace 拥有不同的资源。进程所需的资源可以存在与多个 namespace 中，例如 IDs、host-names、user Ids、file names 和一些与网络访问以及进程间通讯的资源都有各自的namespace。
> Namespace 是 Linux 容器的一个基础。
> 术语 namespace 经常被使用于表达一种资源的命名空间（例如进程 ID）以及特定名称的空间。
> Linux 系统启动时为每种类型的资源都创建了单个 namespace，供所有进程使用。进程可以创建额外的 namespace，也可以加入不同的 namespace。

## Linux 系统中 namespace 的分类

| 命名空间                          | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| Mount (mnt)                       | 拥有一个独立的挂载点列表，并对该 namespace 中的进程可见。这意味着您可以在 mount namespace 中挂载和卸载文件系统，而不会影响主机文件系统。 |
| Process ID (pid)                  | 将一组 PID 分配给独立于其他 namespace 中的一组 PID 的进程。在新的 namespace 中创建的第一个进程分得 PID 1，子进程被分配给后续的 PID。如果子进程使用自己的 PID namespace 创建，则它在该 namespace 中使用 PID 1，在父进程的 namespace 中使用自己的 PID。 |
| Network (net)                     | 拥有独立的网络栈：自己的专用路由表、IP 地址集、套接字列表、连接跟踪表、防火墙及其他网络相关资源。 |
| Inter-process Communication (ipc) | 有自己的 IPC 资源，例如 [POSIX 消息队列](https://man7.org/linux/man-pages/man7/mq_overview.7.html)。 |
| UTS                               | 允许单个系统对不同的进程显示不同的主机名和域名。             |
| User ID (user)                    | 拥有自己的一组用户 ID 和组 ID，用于分配给进程。这意味着进程可以在其 user namespace 中拥有 `root` 权限，而不需要在其他 user namespace 中获得。 |

参考 [Namespace 和 Cgroup 的简介及其工作原理](https://www.nginx-cn.net/blog/what-are-namespaces-cgroups-how-do-they-work/#)
