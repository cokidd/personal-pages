---
title: PXE-BOOT-批量安装系统
date: 2020-10-29 10:03:44
tags: Install System
categories: Linux
---

## 1.概述

### 1.1 PXE

PXE （preboot execute environment,预启动执行环境），工作于Client/Server（C/S）的网络模式，支持工作通过远端服务器下载映像，并由此支持通过网络启动操作系统，在启动过程中，终端要求服务器分配IP地址，再用TFTP(trivial file transfer protocol) 或MTFTP(multicast trivial file transfer protocol)协议下载一个启动软件包到本机内存中执行，并由这个启动软件包完成终端（客户端）基本软件设置，从而引导预先安装在服务器中的终端操作系统。

### 1.2 PXE工作过程

(1) PXE Client 设置PXE以启动项启动，Client向网络中DHCP服务器获取IP

(2) DHCP服务器返回分配给Client端IP和PXE文件的位置

(3) PXE Client 向网络中TFTP获取pxelinux.0文件

(4) PXE Client获取pxelinux.0文件后执行该文件

(5) 从服务器中加载内核和文件系统

(6) 进入安装界面

