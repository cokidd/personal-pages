---
title: CentOS磁盘及文件系统挂载
date: 2020-11-02 11:46:30
tags: Base Setting
categories: Linux
---

1.动态磁盘挂载

(1)总线检索获取磁盘

* 查看主机总线

  ```shell
  ls /sys/class/scsi_host
  host0 host1 host2
  ```

* 扫描总线添加设备

  ```shell
  echo "- - -" > /sys/class/scsi_host/host0/scan
  echo "- - -" > /sys/class/scsi_host/host1/scan
  echo "- - -" > /sys/class/scsi_host/host2/scan
  ```

(2)手动添加

* 修改/proc/scsi/scsi文件

  ```shell
  # x 表示硬盘所在SCSI控制器编号(单SCSI控制器为0)
  # y 表示硬盘所在SCSI通道的编号(单通道为0，多通道为具体通道)
  # z 表示硬盘所在SCSI ID号(通过磁盘所在的SLOT来判断)
  # u 表示硬盘所在LUN号(通常默认情况是0)
  echo "scsi add-single-device x y z u" > /proc/scsi/scsi
  ```

2.文件系统的挂载

​	(1)手动挂载

* mount命令进行手动挂载

  ```shell
  #mount <操作> <挂载点> <目标>
  mount /dev/sda /
  ```

* mount命令操作描述
  * -a 挂载 fstab 中的所有文件系统
  * -f 不实际加载设备。查看mount设备
  * -F 与-a同时使用。/etc/fstab中的设备同时加载，可加快执行速度。
  * -T <路径>，/etc/fstab的替代文件
  * -h --help,显示帮助
  * -l 列出带标签的挂载
  * -n 不写入/etc/mtab
  * -o 挂载选项列表
  * -r 以只读方式挂载文件系统
  * -t 限制文件系统的种类集合
  * -v 打印当前进行的操作
  * -V 显示版本信息并退出
  * -w 以读写方式挂载文件系统（默认操作）
  * -B 挂载其他位置的子树(同 -o bind)
  * -M 将子树移动到其他位置
  * -R, --rbind挂载其他位置的子树及其包含的所有挂载
  *  --make-shared 将子树标记为 共享
  * --make-slave 将子树标记为 从属
  * --make-private 将子树标记为 私有
  *  --make-unbindable 将子树标记为 不可绑定
  *  --make-rshared递归地将整个子树标记为 共享
  * --make-rslave 递归地将整个子树标记为 从属
  * --make-rprivate递归地将整个子树标记为 私有
  * --make-runbindable递归地将整个子树标记为 不可绑定

* 挂载点描述

  * -L LABEL=<标签> 按文件系统标签指定设备
  * -U UUID=<uuid>按文件系统uuid指定设备
  * PARTLABEL=<标签>按分区标签指定设备
  * PARTUUID=<uuid>按分区 UUID 指定设备
  * <设备>按路径指定设备
  * <目录>绑定挂载的挂载点
  * <文件>用于设置回环设备的常规文件

* 示例

  ```shell
  mount -o codepage=936,iocharset=cp936 /dev/hda7 /mnt/cdrom (mount -t vfat -o iocharset=cp936 /dev/hda7 /mnt/cdrom)
  mount -o iocharset=cp936 /dev/hda7 /mnt/cdrom
  mount -o loop /abc.iso /mnt/cdrom
  mount /dev/fd0 /mnt/floppy
  mount -t vfat /dev/sdd1 /mnt/usb
  ```

  