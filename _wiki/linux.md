---
layout: wiki
title: Linux
categories: Linux
description: Linux 指令及使用技巧汇总
keywords: Linux
---

## 指令


| 功能                      | 指令                                |
|:--------------------------|:--------------------------------------|
| 解压文件（到当前目录）     | tar -zxvf 文件名                     |
| 解压文件（到指定目录） | tar -zxvf 文件名 -C 指定目录                           |
| 更改文件用户组                     | chown 用户 文件（若包换文件夹下所有用-R）                     |
| history    | 显示历史命令                |
| 创建多个目录           | mkdir -p /usr/local/test/bin             |
| 查看文件末最后几行           | tail -f -10 logs(-f不停地去读最新的内容，有实时监视的效果)             |
| 下载          | wget  download_url          |
| 递归删除         | rm -r 文件夹       |
| 查看进程         | ps -ef |grep name or port        |
| kill 进程         | kill -9 pid       |



