---
title: GridSE
date: 2024-12-18 19:07:02
tags: 
- DEES
- Preﬁx Search
categories:
- DEES论文汇报 

---

# GridSE: Towards Practical Secure Geographic Search via Prefix Symmetric Searchable Encryption



## 论文背景

随着物联网 (IoT) 设备的不断部署以及下一代无线和移动通信的到来，对地理信息系统 (GIS)，特别是基于位置的服务 (LBS) 和地理搜索 (GS) 的需求在各种上下文感知应用中空前高涨。

为了提供准确的 LBS 服务，GS 应用通常需要收集细粒度的个人信息，例如用户的实时位置和兴趣。出于隐私考虑，存储在 GS 服务器（通常托管在云端）中的数据应得到良好保护，例如通过加密，作为数据泄露事件发生前的预防措施。因此，安全地理搜索 (SGS) 成为一种很有前景的数据保护技术，它支持对加密地理数据进行地理搜索。

现有的地理空间（GS）系统，已经深深植根于一种被称为离散全球网格（DGG）的技术。DGG 使用网格的层次化镶嵌技术递归地将地球表面划分为几何单元，然后对这些单元进行编码。一系列 DGG 单元代码描述了一个具有逐步细化分辨率的分层分区。

![image-20241218194223653](/images/image-20241218194223653.png)

如上图所示，单元代码“dr5r7”表示“dr5r”网格内自由女神像周围的一个单元（子区域）。DGG 系统 (DGGS) 还可以提供索引/API 来支持各种 GIS/LBS 服务，例如地理搜索、距离比较和空间可视化。在现实世界系统中，DGGS 技术，例如 Niemeyer的Geohash、Google S2和 Uber H3，已被广泛采用。
