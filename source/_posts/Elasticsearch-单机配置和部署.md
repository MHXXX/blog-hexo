---
title: Elasticsearch 单机配置和部署
date: 2020-12-24 17:18:21
tags:
categories: Elasticsearch
---

# 单机配置

系统配置

```shell
vi /etc/sysctl.conf
    vm.max_map_count=262144
    
vi /etc/security/limits.conf
    *        soft   nproc  65536
    *        hard   nproc  65536
    *        soft   nofile  65536
    *        hard   nofile  65536

vi /etc/security/limits.d/20-nproc.conf
    *          soft    nproc     65536

重启系统
```

elasticsearch.yml
```yaml
cluster.name: my-application
node.name: node-1
network.host: 0.0.0.0
http.port: 9200
cluster.initial_master_nodes: ["node-1"]
node.master: true
node.data: true
http.cors.enabled: true
http.cors.allow-origin: "*"
cluster.max_shards_per_node: 20000
```
jvm.options
```javascript
默认内存太小的话会导致程序报错

{"error":{"root_cause":[{"type":"circuit_breaking_exception","reason":"[parent] Data too large, data for [<http_request>] would be [1039994134/991.8mb], which is larger than the limit of [1020054732/972.7mb], real usage: [1039984088/991.8mb], new bytes reserved: [10046/9.8kb], usages [request=0/0b, fielddata=119730528/114.1mb, in_flight_requests=10046/9.8kb, model_inference=0/0b, accounting=3024996/2.8mb]","bytes_wanted":1039994134,"bytes_limit":1020054732,"durability":"PERMANENT"}],"type":"circuit_breaking_exception","reason":"[parent] Data too large, data for [<http_request>] would be [1039994134/991.8mb], which is larger than the limit of [1020054732/972.7mb], real usage: [1039984088/991.8mb], new bytes reserved: [10046/9.8kb], usages [request=0/0b, fielddata=119730528/114.1mb, in_flight_requests=10046/9.8kb, model_inference=0/0b, accounting=3024996/2.8mb]","bytes_wanted":1039994134,"bytes_limit":1020054732,"durability":"PERMANENT"},"status":429}

-Xms32g
-Xmx32g
```

# linux上部署
- 目前新版本的elasticsearch自带java运行环境,所以不需要配置JAVA_HOME
- 先从[官网](https://www.elastic.co/cn/)下载最新的安装包并解压
- 创建一个非 root 用户(例如es)并清除密码,并将解压后的目录赋给elastic
- 调整`vm.max_map_count`的值为`262144`
- 进入elasticsearch的bin目录运行`./elasticsearch` 直到能成功运行
- 编写开机自启脚本,需要使用`su -c "elasticsearch -d" {user}`使其后台运行