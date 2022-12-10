# <center> what's reactor</center>

reactor即事件驱动处理模型，即一种基于事件驱动的程序设计方式。比如netty就是一个典型的主从 reactor 多线程处理模型。里面有 accept、read、write 事件，所有的连接 Channel/Socket 都注册在一个 selector 上，boss 线程不断地检测 Channel/Socket 看哪个事件发生了，并将该 Channel/Socket 交给对应的 worker线程池进行处理。避免了CPU 空等、避免了大量线程创建销毁切换，极大地压榨了CPU 提高了吞吐量。