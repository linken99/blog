# <center> what's reactor</center>

reactor事件驱动处理模型，是一种基于事件驱动的程序设计方式，一个线程专门监视所有的连接对象看有哪个事件发生了，如果发生了则交给对应的事件处理器（通常在另外一个线程池）处理。如netty就是一个典型的主从 reactor 多线程处理模型。里面有 accept、read、write 事件，所有的连接 Channel/Socket 都注册在一个 selector 上，boss 线程不断地检测 Channel/Socket 看哪个事件发生了，并将该 Channel/Socket 交给对应的 worker线程池进行处理。避免了CPU 空等、避免了大量线程创建销毁切换，极大地压榨了CPU 提高了吞吐量。
