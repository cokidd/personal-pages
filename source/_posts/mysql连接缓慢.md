---
title: mysql连接缓慢
date: 2020-12-11 16:18:08
tags: linux
---

内网服务器中mysql中会遇到由于内网干扰导致的连接缓慢的问题。可以如下解决:

编辑/etc/my.cnf[mysql]条目下添加:

```shell
[mysql]
skip-name-resolve
```

