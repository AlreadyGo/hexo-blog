---
title: es模块配置-集群
date: 2017-11-08 20:30:22
tags: [es,elasticsearch]
---
配置分为static和dynamic两种,
static配置在elasticsearch.yml,环境变量,或者在启动节点的命令行中;
dynamic可以通过cluster-update-settings api动态更新.
<!-- more -->

***
# 集群
主节点主要功能之一是决定哪个分片分配到哪个节点,什么时候在节点间移动分片来平衡集群
***
## 分片分配配置
### cluster.routing.allocation.enable:all
什么样的分片可以被分配
- all 
- primaries 仅分配主分片
- new_primaries 仅分配新建索引的主分片
- none 不允许分配分片
### cluster.routing.allocation.node_concurrent_recoveries:2
一个节点上并发恢复分片的数目
### cluster.routing.allocation.node_initial_primaries_recoveries:4
并发从本地磁盘恢复分片的数目
### cluster.routing.allocation.same_shard.host:false
同一台机器有多个节点的时候,检查同一分片是否重复分配
### indices.recovery.concurrent_streams:3
### indices.recovery.concurrent_small_file_streams:2
***
## 分片平衡配置
### cluster.routing.rebalance.enable:all
是否平衡分片
- all
- primaries 仅平衡主分片
- replicas 仅平衡副本分片
- none
### cluster.routing.allocation.allow_rebalance:indices_all_active
什么时候允许平衡
- always 
- indices_primaries_active 当集群中所有的主分片都分配好了
- indices_all_active 当集群中所有主分片和副本分片都分配好了
### cluster.routing.allocation.cluster_concurrent_rebalance:2
集群允许并发平衡分片的数目
***
## 分片会被放在哪儿?
不管平衡算法如何,forced awareness或者allocation filtering的优先级更高
### cluster.routing.allocation.balance.shard:0.45f
单个节点上分片分配的权重
### cluster.routing.allocation.balance.index:0.55f
单个节点每个索引分片数目的权重
### cluster.routing.allocation.balance.threshold:1.0f
平衡门槛,值越高越不积极去平衡分片
***
## 基于磁盘的分片分配
### cluster.routing.allocation.disk.threshold_enabled:true
是否开启磁盘分配
### cluster.routing.allocation.disk.watermark.low:85%
磁盘检查低水位线,一旦磁盘使用率超过这个水位线,es将不会给该节点分配新的分片,可以设置百分比也可以设成绝对数值
### cluster.routing.allocation.disk.watermark.high:90%
磁盘检查高水位线,一旦结果超过这个水位线es会尝试把该节点上的分片重新分配到其他节点
### cluster.info.update.interval:30s
磁盘检查时间间隔
### cluster.routing.allocation.disk.include_relocations:true
当计算磁盘使用率的时候是否把正在重新分配的分片考虑进来
***
## 分片分配感应
### cluster.routing.allocation.awareness.force.zone.values: zone1,zone2
正常的感应,如果一个zone联系不上其他zone,同一分片还是会分配到同一个zone上;配置了这个参数后,同一主分片和副本分片不允许分配到同一zone上
### cluster.routing.allocation.awareness.attributes: rack_id,zone
启动集群时,指定rack_id,当使用感应属性时,没有该属性的节点将不会被分配分片

> ./bin/elasticsearch --node.rack_id rack_one

感应属性,可设多个值,逗号分隔,执行search或者get操作时,会优先搜索感应群组中的分片,这样通常会更快;当有多个机架时,es会移动分片来保证同一机架内不会存在同一索引的同一主副分片;
***
## 分片过滤
### cluster.routing.allocation.include.{attribute}
分配索引到满足条件的节点(逗号分隔,至少满足一个)
### cluster.routing.allocation.require.{attribute}
分配索引到满足条件的节点(逗号分隔,全部满足)
###  cluster.routing.allocation.exclude.{attribute}
分配索引到不满足条件的节点
### attribute的取值,支持通配符
- _name 节点名
-  _ip 节点ip
- _host 节点主机名
## 其他集群配置
### cluster.blocks.read_only:false
整个集群只可读,不能创建删除索引
### logger开头的配置
用来管理日志,比如logger.indices.recovery

