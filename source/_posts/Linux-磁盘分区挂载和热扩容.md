---
title: Linux 磁盘分区挂载和热扩容
date: 2021-01-19 10:19:40
tags: [Linux,磁盘分区,磁盘扩容]
categories: Linux
---



1. 查看磁盘信息
```shell
fdisk -l
```

2. 对磁盘进行分区
```shell
fdisk /dev/sdc

#1.创建分区 n
#2.主分区 e
#3.分区号 1
#4.起始块和终止块
#5.查看分区 p
#6.写入分区表 w
```

3. 格式化分区
```shell
mkfs -t ext4 /dev/sdc1
```
4. 挂载磁盘
```shell
mount /dev/sdc1 /{dir}
```
5. 查看磁盘UUID
```shell
sudo blkid
```
6. 开机自动挂载
```shell
vim /etc/fstab
{disk}  {targetDirector}  ext4  defaults  0  0
```

7. 分区热扩容   
```Shell
以 /dev/vdb 为例,只有一个主分区 /dev/vdb1

growpart /dev/vdb 1

resize2fs /dev/vdb1
```