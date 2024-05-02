# 原理解析

## `NetLink`基础概念

`NetLink`是一种IPC(Inter-Process Communication)机制，允许进程间通信。`NetLink`是一种基于`SOCK_RAW`套接字的通信方式，允许进程之间通过网络通信。`NetLink`是一种轻量级的通信方式，适用于需要快速、高效的通信场景。

`NetLink`的优点包括：

    * 轻量级：`NetLink`是一种基于`SOCK_RAW`套接字的通信方式，不需要任何额外的资源开销，适用于需要快速、高效的通信场景。
    * 可靠性：`NetLink`通过使用`NETLINK_KOBJECT_UEVENT`协议，可以确保消息的可靠性，即使网络故障或进程崩溃，也能保证消息的传递。
    * 可扩展性：`NetLink`可以扩展到多个进程间通信，适用于需要多个进程之间进行通信的场景。
    * 安全性：`NetLink`通过使用`NETLINK_KOBJECT_UEVENT`协议，可以确保消息的安全性，防止进程间通信的安全问题。
    * 灵活性：`NetLink`可以支持多种通信协议，包括`NETLINK_KOBJECT_UEVENT`、`NETLINK_GENERIC`等，适用于不同的通信场景。
    * 兼容性：`NetLink`可以兼容多种操作系统，包括Linux、Windows等，适用于不同的操作系统。
    * 可维护性：`NetLink`可以轻松地扩展和修改，适用于需要快速迭代和修改的场景。
    * 双向全双工异步传输：`NetLink`支持双向全双工异步传输，适用于需要双向通信的场景。
    * 支持组播传输：`NetLink`支持组播传输，适用于需要组播通信的场景。

