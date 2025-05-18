+++
date = '2025-05-18T11:24:36+08:00'
draft = false
title = 'LevelDB 概览'
tags = ['storage', 'cpp']
categories = ['leveldb']
+++

## 1. Introduction
leveldb 是一个持久化的、最早一批实现 LSM-tree 模型的 kv 数据库，读写性能都很高（当时），并且整体实现也较为简洁。

## 2. Design Overview
### 2.1. Assumption
> 影响到为什么选择了 LSM-Tree 模型，而不是其他的（还有啥呢？）。

目前只知道是当时磁盘顺序写比随机写快很多。  
还有有序性，例如前缀压缩就是基于有序性做的。  
再补充
### 2.2. Interface
作为 KV 存储系统：
1. 提供 Put, Get, Delete 常规接口
2. 提供 writebatch 接口供 atomic update，此外还支持 sync write、snapshot
3. 提供迭代器用于遍历
4. 用户可自定义比较器，定义 key 的顺序
### 2.3. Architecture
leveldb 以 library 的方式链接到用户程序中使用，而不是 client-server 的方式。  

核心组件包括 memtable, log, sstable, manifest, env：
- LOG：记录变更记录，包括 write 和 delete。主要用于保证异常情况不丢失数据
- memtable：in-memory 有序 key-value 结构，用于存储用户最新的 key-value pairs
- sstable：on-disk immutable 有序 key-value 结构，memtable 满足某些条件（例如大小超过阈值）时会转化成 sstable。sstable 内部会分成多个 level，低层（更新）sstable 也会定期整理生成高层 sstable。每个 sstable 对应一个文件，存储的 key-value 可能会有重叠。
- env: 抽象层，对不同平台提供的接口做了统一封装，使得能跨平台使用。
![Arch](./arch.jpg)

可以从以下几个方面了解 leveldb：
1. layout
2. wal 流程
3. compaction 流程
4. iterator 设计
5. recovery 流程
6. MVCC
