---
title: PXE-BOOT-批量安装系统
date: 2020-10-29 10:03:44
tags: Install System
categories: Linux
---

## 概述

### PXE

PXE （preboot execute environment,预启动执行环境），工作于Client/Server（C/S）的网络模式，支持工作通过远端服务器下载映像，并由此支持通过网络启动操作系统，在启动过程中，终端要求服务器分配IP地址，再用TFTP(trivial file transfer protocol) 或MTFTP(multicast trivial file transfer protocol)协议下载一个启动软件包到本机内存中执行，并由这个启动软件包完成终端（客户端）基本软件设置，从而引导预先安装在服务器中的终端操作系统。

### PXE工作过程

(1) PXE Client 设置PXE以启动项启动，Client向网络中DHCP服务器获取IP

(2) DHCP服务器返回分配给Client端IP和PXE文件的位置

(3) PXE Client 向网络中TFTP获取pxelinux.0文件

(4) PXE Client获取pxelinux.0文件后执行该文件

(5) 从服务器中加载内核和文件系统

(6) 进入安装界面

### PXE网络安装

```shell
yum -y install syslinux xinetd tftp-server
mkdir /var/lib/tftpboot/pxelinux.cfg
#pxelinux.0是引导驱动文件
cp /usr/share/syslinux/pxelinux.0 /var/lib/tftpboot/
```

设置网络守护进程服务程序&&FTP服务

```shell
vi /etc/xinetd.d/tftp

#设置守护tftp服务开启，默认为关闭。(未安装tftp服务,不存在当前文件)
disable = no
service tftp
{
        socket_type             = dgram
        protocol                = udp
        wait                    = yes
        user                    = root
        server                  = /usr/sbin/in.tftpd
        server_args             = -s /var/lib/tftpboot <-设置启动目录
        disable                 = yes <- 设置为no
        per_source              = 11
        cps                     = 100 2
        flags                   = IPv4
}

systemctl start xinetd
systemctl enable xinetd
```

设置DHCP服务器

```shell
yum -y install dhcp
vi /etc/dhcp/dhcp.conf
#设置域名
option domain-name     "";
#设置DNS服务器
option domain-name-servers     ;
#设置最小时间
default-lease-time 600;
#设置最大时间
max-lease-time 7200;
#设置为认证有效
authoritative;
# 设置子网ip地址区域，掩码
subnet 10.0.0.0 netmask 255.255.255.0 {
    #设置ip分配区域
    range dynamic-bootp 10.0.0.200 10.0.0.254;
    #设置广播ip地址
    option broadcast-address 10.0.0.255;
    #设置网关
    option routers 10.0.0.1;
    #设置启动文件名
    filename        "pxelinux.0";
    #设置获取tftp服务器IP地址也是pxelinux.0所在位置，也是镜像所在位置
    next-server     ;
}
#启动DHCP服务器
systemctl restart dhcpd
```

挂载镜像到tftp服务器上

```shell
#创建pxe文件夹
mkdir -p /var/pxe/centos7
#创建tftp挂载文件夹
mkdir /var/lib/tftpboot/centos7
#挂载镜像文件至pxe文件夹内
mount -t iso9660 -o loop /home/iso/CentOS-7-x86_64-DVD-1503-01.iso /var/pxe/centos7
#将pxe所需内核和虚拟文件系统拷贝到tftpboot文件系统上
cp /var/pxe/centos7/images/pxeboot/vmlinuz /var/lib/tftpboot/centos7/
cp /var/pxe/centos7/images/pxeboot/initrd.img /var/lib/tftpboot/centos7/
# menu.c32是菜单显示类型
cp /usr/share/syslinux/menu.c32 /var/lib/tftpboot/
vi /var/lib/tftpboot/pxelinux.cfg/default

# create new
timeout 100
default menu.c32 <-配置菜单显示类型

menu title ########## PXE Boot Menu ##########
label 1
   menu label ^1) Install CentOS 7
   kernel centos7/vmlinuz
   #添加内核路径（添加http协议，实际镜像位置)
   append initrd=centos7/initrd.img method=http://10.0.0.30/centos7 devfs=nomount

label 2
   menu label ^2) Boot from local drive
   localboot
```

配置httpd服务

```shell
vi /etc/httpd/conf.d/pxeboot.conf
#设置镜像挂载的pxe文件夹位置，别名为\centos7
Alias /centos7 /var/pxe/centos7
<Directory /var/pxe/centos7>
	#配置当前目录起到连接作用
	Options Indexes FollowSymlinks
	#配置提供IP段地址
	Require ip 127.0.0.1 10.0.0.0/24
</Directory>	
systemctl restart httpd

```

### Kickstart自动安装

1.修改anaconda-ks.cfg或手动编辑xx-ks.cfg文件

```shell
 #生成root密码
 python -c 'import crypt,getpass; \
print(crypt.crypt(getpass.getpass(), \
crypt.mksalt(crypt.METHOD_SHA512)))'
password:
#创建ks路径
mkdir /var/www/html/ks
vi /var/www/html/ks/centos7-ks.cfg

#安装
install
#自动执行每一步骤
autostep
#安装后重启
reboot
#加密算法
auth --enableshadow --passalgo=sha512
#安装源地址
url --url=http://10.0.0.30/centos7/
#安装盘名称
ignoredisk --only-use=sda
#键盘布局
keyboard --vckeymap=jp106 --xlayouts='jp','us'
# 系统本地化
lang en_US.UTF-8
#网络设置
network --bootproto=dhcp --ipv6=auto --activate --hostname=localhost
# root密码（上方显示内容）
rootpw --iscrypted $6$EC1T.oKN5f3seb20$y1WlMQ7Ih424OwOn.....
# 时区
timezone Asia/Shanghai --isUtc --nontp
# 分区和引导方式
bootloader --location=mbr --boot-drive=sda
# 初始化分区
zerombr
clearpart --all --initlabel
# 分区大小
part /boot --fstype="xfs" --ondisk=sda --size=500
part pv.10 --fstype="lvmpv" --ondisk=sda --size=51200
volgroup VolGroup --pesize=4096 pv.10
logvol / --fstype="xfs" --size=20480 --name=root --vgname=VolGroup
logvol swap --fstype="swap" --size=4096 --name=swap --vgname=VolGroup
%packages
#基础计算机节点环境，也可以换成其他自由选择
@^compute-node-environment
@base
@core
@scientifice
%end

#修改centos7-ks.cfg权限文件
chmod 644 /var/www/html/ks/centos7-ks.cfg

vi /var/lib/tftpboot/pxelinux.cfg/default

timeout 100
default menu.c32
menu title ########## PXE Boot Menu ##########
label 1
   menu label ^1) Install CentOS 7
   kernel centos7/vmlinuz
   # 设置ks文件内容
   append initrd=centos7/initrd.img ks=http://10.0.0.30/ks/centos7-ks.cfg
label 2
   menu label ^2) Boot from local drive
   localboot
```

2.可以通过system-config-kickstart命令执行图形界面来生成ks.cfg文件，与安装系统方式类似



