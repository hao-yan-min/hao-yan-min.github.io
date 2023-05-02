---
title: cache实验
date: 2023-05-02 19:16:39
tags: 
- 计算机系统结构
- cache
- 硬件
categories:
- 华科实验
---





## 实验内容

在csim.c提供的程序框架中，编写实现一个Cache模拟器。



### 实现功能

输入：内存访问轨迹（src\traces子文件夹中的*.trace文件）。 操作：模拟缓存相对内存访问轨迹的命中（hit）/缺失行为（miss）。 输出：命中、缺失和（缓存行）脱胎/驱除（基于LRU算法）的总数。 完成csim.c文件的结果能够使用命令行参数产生下面的输出结果。 输出形式如下： linux>` ./csim -s 4 -E 2 -b 4 -t traces/yi.trace` 输出：hits:4 misses:5 evictions:2 即当输入时，你完成的csim.c输出和上述相同的功能(*.trace文件下面有详细信息)。
