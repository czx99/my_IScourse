# 运输层
运输层协议为运行在不同主机上的应用进程提供了逻辑通信功能。  
运输层协议是在端系统中而不是在路由器中实现的。在发送端，运输层将从发送应用程序进程接收到的报文转换成运输层分组，该分组称为运输层报文段。然后，在发送端系统中，运输层将这些报文段传输给网络层，网络层将其封装为网络层分组（即数据报）并向目的地发送。  
网络路由器仅作用于该数据报的网络层字段；即它们不检查封装在该数据报的运输层报文段的字段。  
网络层提供了主机之间的逻辑通信，而运输层为运行在不同主机上的进程之间提供了逻辑通信。  

### 多路复用与多路分解
多路分解：将运输层报文段中的数据交付到正确的套接字工作  
多路复用：在源主机从不同套接字，并为每个数据块封装上首部信息从而生成报文段，然后将报文段传递到网络层，所有这些工作称为多路复用  
运输层多路复用的要求：
1. 套接字有唯一标识符
1. 每个报文段有特殊字段来指示该报文段所要交付的套接字，这些特殊字段是源端口号字段和目的端口号字段

一个UDP套接字是由一个二元组全面标识的，该二元组包含一个目的IP地址和一个目的端口号。  
TCP套接字是由一个四元组（源IP地址，源端口号，目的IP地址，目的端口号）来标识的。

### 无连接运输：UDP
UDP从应用进程得到数据，附加上用于多路复用/多路分解服务的源和目的端口号字段，以及两个其他的小字段，然后将形成的报文段交给网络层。网络层则将该运输层报文段封装到一个IP数据报中，然后尽力而为地尝试将该报文段交付给接收主机。如果该报文段到达接收主机，UDP使用目的端口号将报文段中地数据交付给正确地应用进程。值得注意的是，使用UDP时，在发送报文段之前，发送方和接收方的运输层实体之间没有握手。正因为如此，UDP被称为是无连接的。  
许多程序使用UDP的原因：
1. 关于发送什么数据以及何时发送的应用层控制更为精细
1. 无需连接建立
1. 无连接状态
1. 分组首部开销小

- UDP检验和用于确定当UDP报文段从源到达目的地移动时，其中的比特是否发生改变

### 可靠数据传输原理
自动重传请求（ARQ）协议：  
在计算机网络中，基于重传机制的可靠数据传输协议被称为自动重传请求协议。ARQ协议中还需要另外三种协议功能来处理存在比特差错的情况：  
- 差错检测
- 接收方反馈
- 重传

流水线可靠数据传输协议：允许发送方发送多个分组而无需等待确认。流水线技术对可靠数据传输协议可带来如下影响：
- 增加序号范围，因为每个传输中的分组必须有一个唯一的序号，而且也也许有多个在传输中的未确认报文
- 协议的发送方和接收方两端也许不得不缓存多个分组
- 解决流水线的差错恢复的两种基本方法：
    - 回退N步
    - 选择重传

1. 回退N步：  
在回退N步协议中，允许发送方发送多个分组而不需要等待确认，但它受限于在流水线中未确认的分组数不能超过某个最大允许数N。  
2. 选择重传：  
选择重传协议通过让发送方仅重传那些它怀疑在接收方出错的分组而避免了不必要的重传。这种个别的、按需重传要求接收方逐个地确认正确接收的分组。再次利用窗口长度N来限制流水线中未完成、未被确认的分组数。
    - 窗口长度必须小于或等于序号空间大小的一半

### 面向连接的传输：TCP
TCP被称为是面向连接的，这是因为在一个应用进程可以开始向另一个应用进程发送数据之前，这两个进程必须先互相“握手”，即它们必须相互发送某些预备报文段，已建立确保数据传输的参数。  
TCP提供的是全双工服务：如果一台主机上的进程A与另一台主机上的进程B存在一条TCP连接，那么应用层数据就可以在从进程B流向进程A的同时，也从进程A流向进程B。TCP连接也总是点对点的，即在单个发送方与单个接收方之间的连接。  
三次握手：客户首先发送一个特殊的TCP报文段，服务器用另一个特殊的TCP报文段来响应，最后客户再用第三个特殊报文段作为响应。

### 网络拥塞控制
- 端到端拥塞控制：在端到端拥塞控制方法中，网络层没有为运输层拥塞控制提供显式支持。即使网络中存在拥塞，端系统也必须通过对网络行为的观察来推断之。
- 网络辅助的拥塞控制：在网络辅助的拥塞控制中，路由器向发送方提供关于网络中拥塞控制状态的显示反馈信息。这种反馈可以简单地用一个比特来指示链路中的拥塞情况。对于网络辅助的拥塞控制，拥塞信息从网络反馈到发送方通常有两种方式：  
    1. 直接网络反馈
    1. 经由接收方的网络反馈

### TCP拥塞控制
TCP所采用的方法是让每个发送方根据所感知到的网络拥塞程度来限制其能向连接发送流量的速率。如果一个TCP发送方感知从它到目的地之间的路径上没什么拥塞，则TCP发送方增加其发送速率；如果发送方感知沿着该路径有拥塞，则发送方会降低其发送速率。
- 一个丢失的报文段表意味着拥塞，因此当丢失报文段时应当降低TCP发送方的速率
- 一个确认报文段指示该网络正在向接收方交付发送方的报文段，因此，当对先前未确认报文段的确认到达时，能够增加发送方的速率
- 带宽探测
###### TCP拥塞控制算法
1. 慢启动
1. 拥塞避免
1. 快速恢复