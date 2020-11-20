---
title: CentOS释放缓存
date: 2020-11-03 13:58:23
tags:
---

1.清理Linux系统缓存

```shell
#不释放缓存
echo 0 > /proc/sys/vm/drop_caches
#释放页缓存
echo 1 > /proc/sys/vm/drop_caches
#释放文件节点和目录项缓存
echo 2 > /proc/sys/vm/drop_caches
```

