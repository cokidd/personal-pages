---
title: Centos6启动phddns卡死
date: 2020-12-15 18:19:39
tags:
---



# CentOS开机启动卡死

1.开机点击e键，进入grub菜单

2.选择启动版本，按e键

3.再kernel描述后添加 1

4.按b键，进入single模式

5.使用rpm -qa phddns | grep phddns 查看安装组件

6.使用rpm -e phddns 卸载phddns

原因:是由于异常断电导致的动态域名出现问题，卸载该服务对实际环境无特大影响。