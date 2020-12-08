# 第九章 传输层
- 传输层的任务是提供源端到目的端可靠而有效的数据传输，能屏蔽低层网络的差异
- 介于通信子网和资源子网之间，对高层用户屏蔽了通信的细节
- 提供端到端之间的无差错保证
- TCP传递的数据单位协议是TCP报文段，UDP传送的数据单位协议是用户数据报
## 传输层提供的服务：
- 面向连接的服务：FTP、TELNET等
- 面向非连接的服务：SNMP、DNS等
## 两次握手存在的问题：
- 第一次连接使用阶段中发送的分组由于不可预测的时延出现在了第二次连接中，被当作一个新的分组而接收下来，而真正的分组却被当作重复分组被丢弃掉
## 释放连接：
- 非对称释放：
    - 一方中止连接，则连接即告中断
    - 缺陷：可能导致数据丢失
- 对称释放：
    - A提出中止请求，B同意即中止
    - 正常三次挥手释放
    - 最后的确认TPDU丢失：由于已经超时，主机2也会自动释放连接
    - 应答丢失：重新发送释放请求
    - 应答丢失以及后续的DR丢失
## 传输服务：
- 服务原语：
    - 请求
    - 指示
    - 响应
    - 确认
- 面向连接的网络服务
- 无连接的网络服务
## 端口
- UDP和TCP都使用了与上层接口处的端口与上层的应用进行通信
- 传输协议数据单元头部中要写入源端口号和目的端口号
## TCP，UDP协议
- TCP提供面向连接的服务，但不提供广播或多播服务
- UDP只在IP的数据报服务之上增加了端口的功能和差错检测的功能
- UDP一次交付一个完整的报文
- UDP伪首部仅为了计算校验和
- TCP面向字节流
## 运输连接的三个阶段
- 连接建立，数据传送和连接释放
- TCP三次握手具体过程，三次挥手具体过程（参考PPT）
- 主机A在连接释放后必须等待2MSL的时间：
    1. 保证A发送的最后一个ACK报文段能够到达B
    1. 防止“已失效的连接请求报文段”出现在本连接中
## TCP拥塞控制
- 在建立连接时声明最大可接受段长度
- 滑窗协议
- 慢启动和拥塞避免