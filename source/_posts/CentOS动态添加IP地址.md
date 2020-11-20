---
title: CentOS动态添加IP地址
date: 2020-11-19 19:02:20
tags:
---

## CentOS 6 修改ip地址

### 1.获取网卡uuid

​	uuidgen 网卡名称

### 2.修改/etc/sysconfig/network-script/ifcfg-网卡名称文件（如没有请添加，例子为修改静态ip)

```shell
DEVICE=eth2
TYPE=Ethernet
#填写对应UUID的内容
UUID=
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=none
IPADDR=
PREFIX=
GATEWAY=
DNS1=
DEFROUTE=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=no
NAME="System eth2"
#网卡mac地址
HWADDR=
USERCTL=no


```

### 3.重启网络服务

service network restart

## CentOS 7 修改IP地址

### 1.获取网卡uuid(不要在意网卡名编辑文件后会自动修改)

```shell
nmcli con show
```

### 2.修改/etc/sysconfig/network-script/ifcfg-网卡名称文件,备注同上

```shell
TYPE="Ethernet"
BOOTPROTO="none"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
UUID=
DEVICE="ens34"
ONBOOT="yes"
IPADDR=
PREFIX=
GATEWAY=
DNS1=
IPV6_PEERDNS="yes"
IPV6_PEERROUTES="yes"
IPV6_PRIVACY="no"
```

### 3.重启网络服务

```shell
systemctl restart network
```

