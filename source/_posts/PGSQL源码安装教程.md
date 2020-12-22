---
title: PGSQL源码安装教程
date: 2020-12-22 19:51:32
tags: PGSQL
categories: PGSQL
---

### 1.获取源码包并解压

[postgresql源码地址](https://www.postgresql.org/ftp/source/)  

```
wget https://ftp.postgresql.org/pub/source/v12.0/postgresql-12.0.tar.gz
tar zxvf postgresql-12.0.tar.gz
```

### 2.安装依赖

```
#ubuntu
apt-get install zlib1g 
apt-get install zlib1g-dev
apt-get install libreadline-dev
#CentOS
yum install -y readline-devel
yum install -y zlib-devel
```

### 3.编译安装
```
#进入源码目录下
执行 ./configure --help 然后根据需求进行 ./configure
make && make install
```

### 4.初始化数据库
添加用户
```
adduser postgres
passwd postgres
```
创建并赋权data目录
```
cd /usr/local/pgsql
mkdir data
chown postgres /usr/local/pgsql/data -R
```
配置环境变量
```
vim /etc/profile
```
在最后添加
```
export PATH=PATH:/usr/local/pgsql/bin
export PGHOME=/usr/local/pgsql
export PGDATA=/usr/local/pgsql/data
```
初始化数据库
```
initdb
```
启动和结束数据库服务
```
pg_ctl start
pg_ctl stop
```

### 服务化和开机自启