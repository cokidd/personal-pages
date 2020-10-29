---
title: Linux系统基本设定
date: 2020-10-29 16:56:30
tags: Base Setting
categories: Linux
---

1.修改进程资源限制

(1)修改/etc/systemd/system.conf及/etc/systemd/user.conf

```shell
#表示不限制
DefaultLimitCORE=infinity
DefaultLimitNOFILE=65535
DefaultLimitNPROC=65535
systemctl daemon-reload
```

(2)修改/etc/security/limits.d/20-nproc.conf

```shell
*		soft		nproc		65535
*		hard		nproc 		65535
```

2.ntp 时间设置

```shell
cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

3.修改语言及字体设置

(1) 修改/etc/sysconfig/i18n

```shell
LANG="zh_CN.utf8"
SYSFONT="latarcyrheb-sun16"
```

4.内核优化

(1)预留端口设置，修改/etc/sysctl.conf

```shell
#预留端口设置
net.ipv4.ip_local_reserved_ports = xxxx-xxxx,xxxx-xxxx 
sysctl -p
```

5.设置历史命令格式,修改/etc/profile

```shelll
HISTTIMEFORMAT="%F %T `whoami`"
```

