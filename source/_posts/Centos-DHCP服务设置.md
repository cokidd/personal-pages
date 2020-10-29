---
title: Centos-DHCP服务设置
date: 2020-10-29 10:30:39
tags: Base Service
categories: Linux
---

## 1.概述

### 1.1 DHCP

DHCP(Dynamic Host Configuration Protocol 动态主机配置协议)前身是BOOTP协议，是一个局域网UDP网络协议。使用67(服务端)、68(客户端)端口,其作用是集中管理、分配IP地址，使客户端动态IP地址、Gateway(网关)地址、DNS服务器地址信息。

1.2 DHCP Service工作流程

初次获取IP地址流程

(1) DHCP客户端在局域网中通过广播的方式发送Discover请求报文

(2) DHCP服务端收到Discover报文后，查找本地IP地址池。并将IP地址附带租约期限和其他配置信息，封装为Offer报文发送至DHCP客户端

(3) DHCP客户端在多个Offer报文中选择一个Offer报文进行应答，并向DHCP服务端广播Request请求报文;表明请求的IP地址

(4)DHCP服务端收到Request报文，确认IP地址租约;发送DHCPNACK报文确认租约

续租IP地址流程

(1)当地址使用租期达到50%时，客户端会向服务端广播Request请求报文

(2)当上一次Request请求未收到时，会在租期达到87.5%时，客户端会向服务端广播Request请求报文

1.3 DHCP分配IP地址模式

(1)自动分配方式(Automatic Allocation),DHCP服务端为客户端指定一个永久性的IP地址，一旦DHCP客户端第一次成功从DHCP服务端租用到该IP地址，即可永久使用该IP地址

(2)动态分配方式(Dynamic Allocation),DHCP服务端为客户端指定一个具有时间限制的IP地址，时间到期或客户端明确表示放弃该地址时，改地址可重新被分配

(3)手动分配方式(Manual Allocation),客户端的IP地址是由网络管理员指定的，DHCP服务端只是将指定IP地址分配给客户端

2.DHCP配置(CentOS)

2.1系统基本设置

* Selinux关闭(详细内容查看Selinux设置)

  