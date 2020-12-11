---
title: CentOS过期yum仓库设置
date: 2020-12-11 16:32:50
tags: Linux

---

由于CentOS6.5过期导致CentOS无法使用yum命令

异常状态显示:

```shell
Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
YumRepo Error: All mirror URLs are not using ftp, http[s] or file.
 Eg. Invalid release/
removing mirrorlist with no valid mirrors: /var/cache/yum/extras/mirrorlist.txt
Cannot find a valid baseurl for repo: extras
```

使用包含所有版本的yum仓库

http://vault.centos.org/

将原有yum.repos.d中的repo文件备份

创建新的repo文件编辑内容如下(有关系统的参数请自行修改):

```shell
[base]
name=CentOS-6.5 - Base
failovermethod=priority
baseurl=http://vault.centos.org/6.5/os/$basearch/
gpgcheck=1
gpgkey=http://vault.centos.org/RPM-GPG-KEY-CentOS-6

#released updates
[updates]
name=CentOS-6.5 - Updates
failovermethod=priority
baseurl=http://vault.centos.org/6.5/updates/$basearch/
gpgcheck=1
gpgkey=http://vault.centos.org/RPM-GPG-KEY-CentOS-6

#additional packages that may be useful
[extras]
name=CentOS-6.5 - Extras
failovermethod=priority
baseurl=http://vault.centos.org/6.5/extras/$basearch/
gpgcheck=1
gpgkey=http://vault.centos.org/RPM-GPG-KEY-CentOS-6

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-6.5 - Plus
failovermethod=priority
baseurl=http://vault.centos.org/6.5/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://vault.centos.org/RPM-GPG-KEY-CentOS-6

#contrib - packages by Centos Users
[contrib]
name=CentOS-6.5 - Contrib
failovermethod=priority
baseurl=http://vault.centos.org/6.5/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://vault.centos.org/RPM-GPG-KEY-CentOS-6
```

