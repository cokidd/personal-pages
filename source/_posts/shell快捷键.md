---
title: shell快捷键
date: 2020-11-04 10:33:18
tags:
---

1.获取shell快捷键的说明

```shell
#查看bash说明文档
man bash
#查看C-a以下的内容
/c-a
```

2.shell字符串快捷键

* Ctrl-a 将光标移动至当前行的开头
* Ctrl-e 将光标移动至当前行的行尾
* Ctrl-f 将光标向前移动一个字符
* Ctrl-b将光标向后移动一个字符
* Atl-f 将光标移动至下一个单词的结尾
* Atl-b 将光标移动至上一个单词的开头
* Ctrl-l 清理屏幕,重刷当前行

3.shell历史操作快捷键

* Ctrl-p从历史记录列表中获取上一个命令
* Ctrl-n从历史记录列表中获取下一个命令
* Alt-< 移动至历史第一行
* Alt->移动至历史最后一行
* Ctrl-r反向搜索历史记录
* Ctrl-s正向搜索历史记录
* Alt-p 非增量反向搜索历史记录
* Alt-n 非增量正向搜索历史记录
* Ctrl-Alt-p 插入上一个命令的参数
* Alt-., Alt-_插入上一个命令的最后一个单词
* Alt-Ctrl-e 展开历史的所有列表
* Ctrl-o 执行当前内容，并获取上一行内容
* Ctrl-x Ctrl-e 调用emacs编辑器并将结果作为shell执行

4.shell字符操作

* Ctrl-d 删除
* Ctrl-w 剪切前一个单词
* Ctrl-u 剪切到行首
* Ctrl-k 剪切至行尾
* Ctrl-x 剪切当前位置至行尾
* Alt-d 删除单词
* Alt-\ 删除所有空格
* Ctrl-y 粘贴
* Ctrl-v TAB 输入tab字符
* Ctrl-t 调换前两个字符的位置
* Alt-t 调换前两个单词的位置
* Alt-u 当前或后面的单词大写
* Alt-l 当前或后面的单词小写
* Alt-c 前一个单词大写

5.shell补全操作

* tab 文字补全

* Alt-? 列出可能的文字补全

* Alt-/ 补全文件名

* Ctrl-x / 补全可能的文件名

* Alt-~ 补全用户名

* Ctrl-x ~补全可能的用户名

* Alt-$ 补全变量

* Ctrl-x $补全可能的变量

* Ctrl-x @ 补全可能的主机名

* Alt-! 补全命令

* Ctrl-x !补全可能完成的命令

* Alt-TAB 补全与历史命令对比相似的命令

* Alt-{ 执行文件补全

  

