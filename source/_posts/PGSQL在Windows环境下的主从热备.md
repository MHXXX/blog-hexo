---
title: PGSQL在Windows环境下的主从热备
date: 2020-12-22 19:29:59
tags: PGSQL
categories: PGSQL
---
# 1.配置主库

- 配置`pg_hba.conf`
- 在最后添加 `host replication postgres 0.0.0.0/0 md5`
- 配置`postgresql.conf`,修改如下配置,(如果配置前面带`#`,需要删除`#`)
```
wal_level = replica 
max_wal_senders = 8 
wal_keep_segments = 256 
wal_sender_timeout = 60s
```
- 启动主库

# 2.开始备份
- 连接到主库,执行命令 `select pg_start_backup('Replitionwork');`开启备份
- 关闭备库,清空备库data目录,拷贝主库data目录下的所有文件到备库data目录下,删除`postmaster.pid`文件
- 在主库执行命令`select  pg_stop_backup(),current_timestamp;`关闭备份

# 3.配置备库
- 拷贝备库share目录下`recovery.conf.sample`文件到data目录下,重命名为`recovery.conf`
- 在`recovery.conf`中添加如下配置
```
standby_mode = on 
primary_conninfo = 'host=主库ip port=5432 user=postgres password=主库密码' 
```
- 配置 `postgresql.conf` 文件
```
hot_standby = on 
hot_standby_feedback = on
```
- 启动备库,在主库执行`select client_addr,sync_state from pg_stat_replication;`进行验证

# 4.主备切换
- 备库 `recovery.conf`重命名为`recovery.done`
- 主库按照上面备库配置一遍重启
