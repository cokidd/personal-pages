---
title: Detect-And-Mount-CDROM出错问题解决
date: 2020-10-29 10:39:45
tags: Install System
categories: Linux
---

问题范围:部分Debian及衍生系统
问题产生核心:该问题的核心原因是因为安装系统时，系统文件没有正确挂载至指定位置
解决办法:
1. 当出现该问题时处于安装界面，按Alt+\<F2>进入单用户模式

2. 模拟iso镜像挂载至/cdrom 

  (1) 模拟iso挂载

  * 创建iso目录

    ```shell
    mkdir /iso
    ```

  * 将当前系统盘挂载至/iso目录

    ```shell
    mount /dev/sd* /iso
    ```

  * 将iso目录中镜像挂载至/cdrom目录

    ```shell
    mount /iso/ubuntu* /cdrom
    ```

  (2)将/media目录挂载至/cdrom目录

  * 将/media目录下的文件已ISO9660文件格式挂载至/cdrom

    ```shell
    mount -o loop -t iso9660 /media /cdrom
    ```

3. 按<Ctrl>+D或exit退出命令行，即可安装系统


