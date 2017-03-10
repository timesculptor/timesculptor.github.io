---
layout: wiki
title: Linux
categories: Linux
description: Linux 指令及使用技巧汇总
keywords: Linux
---

## 常用指令


| 功能                      | 指令                                |
|:--------------------------|:--------------------------------------|
| 解压文件（到当前目录）     | tar -zxvf 文件名                     |
| 解压文件（到指定目录） | tar -zxvf 文件名 -C 指定目录                           |
| 更改文件用户组                     | chown 用户 文件（若包换文件夹下所有用-R）                     |
| history    | 显示历史命令                |
| 创建多个目录           | mkdir -p /usr/local/test/bin             |
| 查看文件末最后几行      | tail -f -10 logs(-f不停地去读最新的内容，有实时监视的效果)   |
| 下载          | wget  download_url          |
| 递归删除         | rm -r 文件夹       |
| 查看进程         | ps -ef &#124;grep name or port        |
| kill 进程         | kill -9 pid       |
| 查看防火墙状态         | service iptables status       |
| 开启火墙        | service iptables start       |
| 关闭防火墙 (临时)        | service iptables stop       |
| 关闭防火墙 (下次开机一直关闭)        | chkconfig iptables off  |
| 验证查看防火墙是否关闭     | chkconfig --list&#124;grep iptables  |
| 重启     | shutdown -r now |
| 关机     | shutdown -h now |
|统计行数|cat api.log |grep ERR | wc -l|
|jps |显示当前所有java进程pid|
|ps -mp 3630 -o THREAD,tid,time|列出pid包含的所有线程信息及线程id(tid)|
|printf "%x\n” tid|将tid转16进制|











