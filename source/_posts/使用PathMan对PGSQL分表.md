---
title: 使用PathMan对PGSQL分表
date: 2020-12-22 19:21:22
tags: ['PGSQL','PathMan']
categories: PGSQL
---

### 安装pathman
在[pathman的github的release页面](https://github.com/postgrespro/pg_pathman/releases)获取最新的源码包
```
wget https://github.com/postgrespro/pg_pathman/archive/1.5.12.tar.gz
tar zxvf 1.5.12.tar.gz

cd pg_pathman-1.5.12
make install USE_PGXS=1  
```
如果安装过程中 `make install` 出现问题,可能是postgresql环境变量有问题  

### 修改配置

在数据库的 `postgresql.conf` 配置文件中对 `shared_preload_libraries`进行配置
```
shared_preload_libraries = 'pg_pathman'
```
然后重启pgsql

### 创建扩展
**pathman需要在使用的每个库上执行创建语句**
```
CREATE EXTENSION pg_pathman
```

### 使用
常用的函数
- 根据hash进行分区
```
create_hash_partitions(parent_relid     REGCLASS,
                       expression       TEXT,
                       partitions_count INTEGER,
                       partition_data   BOOLEAN DEFAULT TRUE,
                       partition_names  TEXT[] DEFAULT NULL,
                       tablespaces      TEXT[] DEFAULT NULL)
```
- 根据范围进行分区
```
create_range_partitions(parent_relid    REGCLASS,
                        expression      TEXT,
                        start_value     ANYELEMENT,
                        p_interval      ANYELEMENT,
                        p_count         INTEGER DEFAULT NULL
                        partition_data  BOOLEAN DEFAULT TRUE)

create_range_partitions(parent_relid    REGCLASS,
                        expression      TEXT,
                        start_value     ANYELEMENT,
                        p_interval      INTERVAL,
                        p_count         INTEGER DEFAULT NULL,
                        partition_data  BOOLEAN DEFAULT TRUE)

create_range_partitions(parent_relid    REGCLASS,
                        expression      TEXT,
                        bounds          ANYARRAY,
                        partition_names TEXT[] DEFAULT NULL,
                        tablespaces     TEXT[] DEFAULT NULL,
                        partition_data  BOOLEAN DEFAULT TRUE)
```
- 分区后进行数据迁移
```
partition_table_concurrently(relation   REGCLASS,
                             batch_size INTEGER DEFAULT 1000,
                             sleep_time FLOAT8 DEFAULT 1.0)
```
- 自动创建分区(仅range可以使用)
```
set_auto(relation REGCLASS, value BOOLEAN)
```
