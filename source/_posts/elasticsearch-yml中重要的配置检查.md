---
title: elasticsearch.yml中重要的配置检查
date: 2017-11-13 20:33:27
tags: [es,elasticsearch]
---
启动集群最小的配置
### path.data 和 path.logs
数据和日志文件路径
```
path:
  logs: /var/log/elasticsearch
  data: /var/data/elasticsearch
```
设置多个路径
```
path:
  data:
    - /mnt/elasticsearch_1
    - /mnt/elasticsearch_2
    - /mnt/elasticsearch_3
```
### cluster.name
集群名
```
cluster.name: logging-prod
```
### node.name
节点名
```
node.name: ${HOSTNAME}
```
### bootstrap.memory_lock
锁定jvm只用内存,不使用硬盘
```
bootstrap.memory_lock:true
```
### network.host
默认为127.0.0.1
```
network.bind_host: 0.0.0.0                #监听所有ip的请求
network.publish_host: _bond1:ipv4_         #发布给集群中其他节点知道的地址
```
### discovery.zen.ping.unicast.hosts
集群中其他节点ip:tcp端口,用来联系集群中其他节点
```
discovery.zen.ping.unicast.hosts:
  - 192.168.1.10:9300
  - 192.168.1.11 
  - seeds.mydomain.com 
```
### discovery.zen.minimum_master_nodes
最小主节点数目,为了防止脑裂,需要设置发现超过半数的主节点候选者才能组成集群
```
discovery.zen.minimum_master_nodes: 2
```
