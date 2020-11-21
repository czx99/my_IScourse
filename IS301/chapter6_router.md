# 第六章：路由选择与网络拥塞控制
- 网络层是处理端到端传输的最低层
- 网络层的任务
    1. 通信子网的拓扑结构
    1. 路由选择方法
    1. 流量和拥塞控制
    1. 网络互联
- 向传输层提供的服务
    1. 面向连接的服务
    1. 无连接的服务
- 通信子网提供的服务(面向连接或无连接)与通信子网结构(虚电路或数据报)没有必然联系
## 路由选择算法
- 路由选择的实现：路由表
    - 路径选择的原则是使到达目的节点的链路数最少
    - 虚电路使用的路由表
- 静态路由算法
    - 随机路由选择：是由收到分组的节点随机地选择一个出口转发出去；缺点是可能造成某些分组长期在通信子网中中转，到达不了目的节点
    - 洪泛算法：把收到的每一个包，向除了该包到来的线路外的所有输出线路发送
        - 产生大量重复包
        - 每个包头包含站点计数器，每经过一站计数器减1，为0时则丢弃该包
    - 固定式路由选择
        - 固定式单路由算法
        - 固定式多路由算法
- 动态路由选择
    - 孤立的自适应路由算法：仅仅依据自身当前运行状态信息来决定路由，而与网络其他各节点的状态无关
        - 最短排队等待法（“热土豆”法）
        - 最短排队加偏法
    - 分布式自适应路由选择：在网络相邻节点之间，每经过一定时间相互交换一次状态信息，各节点根据相邻节点送来的状态信息来修改自己的路由表
        - 距离矢量路由算法：每个路由器维护一张矢量表，表中列出了到每个目的地已知的最佳距离和路径
        - 状态链路路由算法：全局路由选择算法。每个节点将链路状态包向网络中所有其他的节点广播
## 流量控制
- 拥塞控制和流量控制的关系
    - 拥塞控制必须使得通信子网能够传送所有待传送的数据，它是一个全局性问题
    - 流量控制是与某发送者和某接收者之间的点到点业务量有关。它的任务是确保一个快速的发送者不能以比接收者能承受的速率更高的速率传输数据
    - 简单地说，流量控制是防止网络拥塞的一种机制
- 流量控制的主要功能
    - 防止由于网络和用户过载而产生的吞吐量降低及响应时间过长
    - 避免死锁
    - 在用户之间合理分配资源
    - 网络及其用户之间的速率分配
- 流量控制是发送方和接收方之间的传输速率上的匹配，为使没有得到确认的PDU在超时后能够重发，通常必须在缓冲区中暂存
- 流量整形技术
    - 漏桶算法
    - 令牌漏桶算法
    - TCP滑动窗口协议只是限制一次传送数据的数量，而不是传输的速率
## 网络拥塞控制
- 存储-转发死锁
- 重装死锁
- 处理拥塞的方法
    - 分组删除：如果一个节点上出现分组的过度聚集就丢弃其中的一部分
    - 流量控制
    - 缓冲分配
    - 抑制分组
- 宽带网络中的拥塞控制机制
    - 开环拥塞控制：就是在设计网络时事先将有关发生拥塞的因素考虑周到，力求网络在工作时不产生拥塞
    - 闭环拥塞控制：
        - 检测网络系统以便检测到拥塞在何时、何处发生
        - 将拥塞发生的信息传送到可采取行动的地方
        - 调整网络系统的运行以解决出现的问题
- 源算法和链路算法
    - 源算法在主机和网络边缘设备中执行，主要作用时根据反馈信息调整发送速率
    - 链路算法在网络内部执行，主要作用是检测网络拥塞的发生，生成拥塞反馈信息
- TCP流量控制（利用滑动窗口实现流量控制）
    - 数据接收窗口、拥塞控制窗口
    - 拥塞窗口的大小由网络的带宽决定，发送端所能发送的数据段的数量是由接收窗口和拥塞窗口二者的最小值决定
    - 慢启动
    - 拥塞避免
    - TCP发送者启动一个长度为1的拥塞窗口。对于每个收到的ACK。TCP指数增长C-window的大小，直到等于阈值，然后进入拥塞避免阶段。在此阶段中，它继续要线性增加它的C-window窗口，直至达到接收者的最大接收窗口。如果TCP链路丢失了一个数据段，发送端会因收不到ACK帧而超时。这时，发送端将阈值改为C-window窗口大小的一半，再将C-window的值设置为1
- 快重传和快恢复
    - 快重传算法首先要求接收方每收到一个失序的报文段后就立即发出重复确认。这样做可以让发送方及早知道有报文段没有到达接收方
    - 发送方只要一连收到三个重复确认就应当立即重传对方尚未收到的报文段
    