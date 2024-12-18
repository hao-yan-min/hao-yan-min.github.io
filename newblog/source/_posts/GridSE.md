---
title: GridSE
date: 2024-12-18 19:07:02
tags: 
- DEES
- Preﬁx Search
- USENIX Security Symposium
categories:
- DEES论文汇报 

---

# GridSE: Towards Practical Secure Geographic Search via Prefix Symmetric Searchable Encryption

---
汇报人：严浩铭


---



## BACKGROUND

随着物联网 (IoT) 设备的不断部署以及下一代无线和移动通信的到来，对地理信息系统 (GIS)，特别是基于位置的服务 (LBS) 和地理搜索 (GS) 的需求在各种上下文感知应用中空前高涨。

为了提供准确的 LBS 服务，GS 应用通常需要收集细粒度的个人信息，例如用户的实时位置和兴趣。出于隐私考虑，存储在 GS 服务器（通常托管在云端）中的数据应得到良好保护。因此，安全地理搜索 (SGS) 成为一种很有前景的数据保护技术，它支持对加密地理数据进行地理搜索。

现有的地理空间（GS）系统，已经深深植根于一种被称为离散全球网格（DGG）的技术。离散全球网格系统将地球区域划分为几何网格。每个网格可以进一步递归地划分为更小的区域，这些区域称为单元格，并具有逐渐精细的分辨率。为了便于计算，每个区域或单元格都由一个点表示，并分配一个可识别的字符串，我们将其称为单元格代码。

![图1](/images/image-20241218194223653.png)

如上图所示，单元代码“dr5r7”表示“dr5r”网格内自由女神像周围的一个单元（子区域）。DGG 系统 (DGGS) 还可以提供索引/API 来支持各种 GIS/LBS 服务，例如地理搜索、距离比较和空间可视化。在现实世界系统中，DGGS 技术，例如 Niemeyer的Geohash、Google S2和 Uber H3，已被广泛采用。

在DGGS中，GS数据库通过分层单元码进行索引。地理搜索本质上是在索引单元码和搜索请求之间找到前缀匹配。为了支持SGS，其中索引和搜索请求都被加密，这个问题可以被表述为安全前缀搜索的一般问题。



**问题描述：**

* 理论上，许多的技术，例如全同态加密 (FHE)和多方计算 (MPC)，可以通过对密文进行前缀搜索来实现SGS。然而，这些技术的当前实现当实时性能受到关注时仍然远非实用。

* 为了实现对加密数据的安全搜索，可搜索对称加密 (SSE) 提供了一种通用且可行的解决方案。然而，现有的方案无法实现前缀搜索。少数作品，例如 Moataz 等人，支持子串搜索并提供语义安全的 SSE，但无法解决正向隐私或反向隐私。且方案使用了ORAM方案，导致了导致高通信复杂度。

  

**本文贡献：**

* 构建了一个高效、可扩展且与 DGGS 兼容的 SGS 方案 GridSE，它支持正向和反向隐私。
* 设计了一种新的密码学原语 - 对称前缀谓词加密 (SP2E) - 用于对前缀进行谓词加密。



## PRELIMINARIES



### 地理搜索框架

一个典型的地理信息服务系统主要包含三个主要实体，分别是用户、客户端、服务器。其中服务器是一个提供计算或存储服务的云提供商。客户端是一个 GIS/LBS 提供商，它将数据（加密后）外包给服务器，同时保留一些本地计算和存储能力；用户是发出地理搜索请求的使用者。

![图二：地理搜索框架](/images/截屏2024-12-19 10.27.39.png)

文章将数据库逻辑划分为数据和索引两个部分。数据按照位置进行索引，用于索引位置的键称为索引键。在本工作中，只关注位置属性（例如，与 DGGS 兼容的地理单元代码）以及如何检索其加密形式的精确索引键，以便正确、安全、高效地返回查询的加密块。





### 动态前缀对称可搜索加密

一个动态前缀对称可搜索加密方案包含
