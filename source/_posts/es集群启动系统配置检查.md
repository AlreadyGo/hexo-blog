---
title: es集群启动系统配置检查
date: 2017-11-11 20:32:21
tags: [es,elasticsearch]
---
### 堆大小检查
./bin/elasticsearch文件
```
ES_HEAP_SIZE=20G
```
./conf/jvm.options
```
-Xms2g 
-Xmx2g
```
- 把xms(堆初始化大小)和xmx(最大堆大小)设为相等
- 堆太小oom,堆太大gc停止时间越长
- xmx不要超过机器物理内存的50%
- 因为jvm使用压缩对象指针,不要把xmx设超过临界点,临界点一般在靠近32G,正确的日志如下:
```
heap size [1.9gb], compressed ordinary object pointers [true]
```
<!-- more -->

- 尽量保持在zero-based compressed oops临界点以下,大多数系统临界点在26G是安全的,有一些系统可以达到30G
可以在启动es的时候加上XX:+UnlockDiagnosticVMOptions -XX:+PrintCompressedOopsMode两个jvm参数来检验下是否在临界点之下,会出现如下日志:
```
heap address: 0x000000011be00000, size: 27648 MB, zero based Compressed Oops
```
这段说明zero-based compressed oops生效了,而不是像这样:
```
heap address: 0x0000000118400000, size: 28672 MB, Compressed Oops with base: 0x00000001183ff000
```

### open files 打开文件数 /max user processes 最大用户进程数/max locked memory jvm进程锁定到内存
```
[esadmin]$ ulimit -a
core file size          (blocks, -c) 0
data seg size          (kbytes, -d) unlimited
scheduling priority            (-e) 0
file size              (blocks, -f) unlimited
pending signals                (-i) 515100
max locked memory      (kbytes, -l) unlimited
max memory size        (kbytes, -m) unlimited
open files                      (-n) 65535
pipe size            (512 bytes, -p) 8
POSIX message queues    (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time              (seconds, -t) unlimited
max user processes              (-u) 65535
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```
/etc/security/limits.conf中配置
### 检查mmap count
至少262144
```
$ sysctl vm.max_map_count
vm.max_map_count = 262144
```
在/etc/sysctl.conf中修改vm.max_map_count后,运行sysctl vm.max_map_count
### 检查插件
没有head
```
$ ./bin/plugin list
    - analysis-ik
    - delete-by-query
    - mapper-murmur3
    - mapper-size
    - repository-hdfs
```

