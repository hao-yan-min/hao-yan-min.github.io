---
title: graph survey
date: 2023-05-01 22:59:22
tags: 
- 图学习
- 深度学习
categories:
- 图神经网络
---



# graph survey



## introduction

* Graph ananalytics mainly includes s graph processing, graph mining, and graph learning.(图像处理，图像挖掘，图学习)

* Graph Processing. Conventional grapg algorithms are designed to process graphs iteratively(迭代) until covergence(收敛). ([PageRank算法](https://zhuanlan.zhihu.com/p/137561088)，[单源最短路径算法](https://blog.csdn.net/weixin_41579721/article/details/114552443))

  *  are based on traversal operations and generally focus on performing linear algebra-like computational operations on the graph.

* Graph mining. Graph mining aims to discover specific structures or patterns in graphs.

  * graph mining technology is an ideal tool for dealing with complex data structures because of its complex data object relationships and rich data presentation. 

  * graph mining can be used to discover structure

    content relationships in social media data, to mine

    community-dense subgraphs, to extract network motifs or

    signifificant subgraphs in protein-protein or gene interaction

    networks, to discover 3D motifs in protein structures or

    chemical compounds, etc.

* Graph learning. 

  * graphs can characterize the relationships between everything.
  * Graph neural networks establish a deep learning framework for non-Euclidean spatial data, and compared to traditional network representation learning, it is able to perform deeper information aggregation operations on graph structures than traditional network representation learning models.（图神经网络为非欧几里得空间数据建立了深度学习框架，与传统的网络表示学习相比，它能够比传统的网络表示学习模型对图结构执行更深入的信息聚合作。）

## background

* graph  Terminology

  * The formula for a graph is G =(V,E), where V stands for the vertex set and E for the edge set.  Each vertex and each edge has its attribute value at the same time.

* Domain-Specifific Architecture Types for Graph Analytics.

  FPGA-Based Architecture.

  * consists various types of programmable resources, which enables developers to rapidly prototype application-specifific accelerators using dedicated hardware description languages and reconfigure these accelerators as often as needed. (使开发人员能够使用专用的硬件描述语言快速地原型化特定于应用程序的加速器，并根据需要经常重新配置这些加速器。)

  * offer reconfifigurability at the expense of lowered

    clock frequencies, which is about 10 × lower than that of

    CPUs(降低了时钟的频率)

    

## Software Systems Implementation for Graph Analytics

###  Software Graph Processing Systems

* can be classifified into two main categories: single-machine graph processing systems and distributed graph processing systems.

* According to whether the graph data can be stored in memory during processing, these systems can be divided into in-memory graph processing systems and out-of-core graph processing systems.

* Single-Machine Graph Processing Systems. 
  * Single machine graph processing systems can fully exploit the ability of a single machine to handle graph computation tasks and avoid the expensive network communication overhead in distributed systems. 
  * 统受到固定的硬件资源的限制，无法实现良好的可伸缩性，并且处理时间通常与图形数据的大小成正比。
  * Single-machine in-memory graph processing systems often have multiple cores and support very large memory of more than 1 TB, allowing them to handle graph data with hundreds of billions of edges.  However, single shared memory systems can only scale by increasing the number of CPUs or expanding the memory size.
  * Ligra is a lightweight shared memory-based single machine graph processing system, which provides programming abstraction based on edgeMap function, vertexMap function, and vertexSubset type, simplifying the writing of graph processing algorithms.
  * The key idea of Galois [85] single-machine graph processing system is to fully exploit the benefifits of autonomous scheduling in a data-driven computing mode.
* Distributed Graph Processing Systems.
  * A distributed graph computing system consists of multiple computing nodes, each of which has its own memory and external memory. Therefore, compared to single-machine graph computing systems, distributed graph processing systems are less limited by hardware in terms of scalability.
  * 设计一个合适的数据划分策略是一个挑战。同时，计算节点之间的通信成为性能的瓶颈



### Software Graph Mining Systems

* They search for subgraphs that satisfy the conditions of the algorithm in the input graph G. The process of finding subgraphs can be modeled with a search tree where each node represents a subgraph, and the subgraphs at the k+1 level are expanded from the subgraphs at the k level.

  ![image-20230312162325702](\typora-user-images\image-20230312162325702.png)

  
