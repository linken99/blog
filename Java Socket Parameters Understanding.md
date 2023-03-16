



```java
//BACK_LOG设置队列允许的等待连接数，超过的连接会被拒绝掉
serverSocket.bind(new InetSocketAddress(9090), BACK_LOG);

Socket client = serverSocket.accept();

//为true则会自动发送心跳检测对方是否存活
client.setKeepAlive(true);
client.setOOBInline(false);
//设置读缓冲区的大小
client.setReceiveBufferSize(1024);
client.setReuseAddress(false);
client.setSendBufferSize(1024);
//设置断开连接的速度
client.setSoLinger(false,100);
//优化参数，如果发生方的数据小是否等待并合并多个数据一起发送。true表示不合并立即发送，false表示合并多个再发送
client.setTcpNoDelay(true);
```

7932：socket server pid，lsof查看该进程的文件描述符FD为mem表示该FD是内存共享的(即多个进程共享该FD)，5u是进程新建的FD用来监听的。

![image-20230316180656606](C:\Users\chen\AppData\Roaming\Typora\typora-user-images\image-20230316180656606.png)

##### 参考网址

lsof 之FD文件描述符 https://www.cnblogs.com/aaronax/p/5654652.html
共享内存之-mmap内存映射 https://www.jianshu.com/p/096e1b58c678
Linux四种共享内存技术 https://blog.csdn.net/m0_56145255/article/details/124792742