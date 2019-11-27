---
title: 'es集群滚动重启'
date: 2017-10-22 08:21:48
tags: [es,elasticsearch]
---
## 禁止分片分配
```
curl -XPUT '/_cluster/settings?pretty' -d '{
  "transient": {
    "cluster.routing.allocation.enable": "none"
  }
}'
```
<!-- more -->

## 关闭单个节点
关闭节点 
```
$ kill <pid>
```
## 执行维护/升级等操作
比如替换./plugins下某个文件夹,替换完成后执行./bin/plugin list 查看插件是否正常
```
scp esadmin@172.18.187.2:~/es-ik/elasticsearch-analysis-ik-1.9.3.zip ./
```
## 重启节点
```
$ ./bin/elasticsearch -d
```
## 打开自动分配
```
curl -XPUT '/_cluster/settings?pretty' -d '{
  "transient": {
    "cluster.routing.allocation.enable": "all"
  }
}'
```
## 检查集群是否恢复完成
active_shards_percent_as_number进度是100,负载中的分片relocating_shards,正在被初始化分片initializing_shards及为被分配的分片unassigned_shards等均为0时表示恢复完成
```
curl -XGET '/_cluster/health?pretty'
```
files_percent,bytes_percent,translog_percent进度为100%时恢复完成
```
curl -XGET  '/_cat/recovery?v&human&active_only=true&detailed=true'
```
## 重复步骤1-5

## 注意:检查allocation设置
```
curl -XGET  '/_cluster/settings?pretty&filter_path=**.enable'
```
