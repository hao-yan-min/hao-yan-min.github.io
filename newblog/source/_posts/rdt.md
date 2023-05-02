---
title: rdt
date: 2023-05-01 23:56:10
tags:
- 计算机通信与网络
- 华科实验
---

# RDT实验



## 数据结构

```C++
class GBNReceiver :public RdtReceiver
{
private:
	int expectSequenceNumberRcvd;	// 期待收到的下一个报文序号
	int seqlen;                     //序号宽度
	Packet lastAckPkt;				//上次发送的确认报文

```

```C++
class GBNSender :public RdtSender
{
private:
	int expectSequenceNumberSend;	// 下一个发送序号 
	bool waitingState;				// 是否处于等待Ack的状态
	int base;                       //当前窗口基序号
	int winlen;                     //窗口大小
	int seqlen;                     //序号宽度
	deque<Packet> window;           //窗口队列
	Packet packetWaitingAck;		//已发送并等待Ack的数据包

```

```C++
struct  Packet {
	int seqnum;										//序号 发送方发送报文的序号
	int acknum;										//确认号 接收端发送给发送端的确认序号
	int checksum;									//校验和
	char payload[Configuration::PAYLOAD_SIZE];		//payload 装载的数据
	
```



### GBN协议

![image-20221112172024646](\typora-user-images\image-20221112172024646.png)

* 不等发送方收到确认报文，就继续发送下一组报文。可以连续发送的数量为窗口的大小。
* 如果接受方收到的报文序号和预期序号不同，则发送上一次的确认报文；否则发送新的确认报文
* 如果发送方收到的确认帧在等待确认的数据帧中，则默认该帧及以前的帧都已经收到了，窗口继续滑动。

