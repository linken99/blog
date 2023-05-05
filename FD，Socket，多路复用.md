### FD，Socket，多路复用

fd：文件描述符，打开一个文件需要分配一个fd，连接一个进程/DB/远程服务、监听一个端口、接收一个TCP/UDP连接都要创建一个socket，也要分配一个fd。而一个进程接收/处理的fd是有限制的，超出这个限制的TCP连接会被进程拒绝掉。

socket监听TCP连接，select/poll/epoll监听 socket的状态。

netty的核心在于主从reactor线程模型，这种模式最大限度地榨干了CPU利用率，最及时地处理了用户请求(连接能快速建立、数据准备好就能快速得到处理)，这种模式使得吞吐量高、响应及时，所以netty被广泛使用。

#### 常见的连接问题

连接超时、读取数据超时、连接被拒绝、fd/socket被关闭(用户向关闭掉的socket读写数据时会报unexpected end of stream)

如果对方打印了一些日志且还有SocketTimeout说明是读取超时，即对方处理、传输时间超出了自己设置的