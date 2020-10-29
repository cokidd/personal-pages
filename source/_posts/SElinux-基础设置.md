---
title: SElinux-基础设置
date: 2020-10-29 12:43:56
tags: Base Setting
categories: Linux 
---

## 1.概述

### 1.1 SElinux

SElinux(Security Enhanced Linux 安全强化Linux)，是由美国国家安全局(NSA)联合其他安全机构共同开发的，旨在强化Linux操作系统的安全性，解决传统Linux系统中自主访问控制(DAC)系统中的各种权限问题。

1.2 访问控制的特点

(1)自主访问控制系统(Discretionary Access Control, DAC):是Linux的默认访问控制的方式，也就是依据用户的身份对文件及目录rwx权限判断是否可以访问。

(2)强制访问控制(Mandatory Access Control, MAC):是通过SELinux的默认策略规则来控制特定的进程对系统的文件资源的访问。也就是在root用户权限下，当你访问文件资源时使用了不正确的进程，那么也是不能够访问这个资源的。

1.3 SELinux的运行机制

SELinux本质上是Subject对Object做Action的权限描述,权限描述如下:

* Subject:表示进程主体
* Object:对象表示目标文件
* Action:表示对Object操作描述，如读、写等
* Context:上下文描述Subject的具体信息,如SELinux User、 SELinux Role、SELinux Type、SELinux Level
* SELinux User:Linux到SELinux用的映射，对应的SELinux User都会有相应的Role
* SELinux Role:描述对应SELinuxType规则
* SELinux Type:安全策略SELinux

