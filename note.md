main.cpp->fileserver.h->threadpool.h->myevent.h->utils.h和message.h


### 新文件描述符添加到epoll实例中的作用

将新文件描述符添加到epoll实例中，是网络编程中高效处理多个并发连接的一种常见做法。epoll是Linux内核提供的一种I/O事件通知机制，相比于传统的select和poll机制，epoll在性能上有显著的优势，特别是在处理大量并发连接时。

以下是新文件描述符添加到epoll实例中的具体作用：

1. **高效监控I/O事件**：
   - epoll允许程序监控多个文件描述符（通常是套接字）上的I/O事件（如可读、可写、异常等）。
   - 通过将新文件描述符添加到epoll实例中，程序可以统一管理和监控这些文件描述符上的事件，而无需轮询每个文件描述符的状态，从而提高了效率。

2. **支持边缘触发和水平触发**：
   - 边缘触发（EPOLLET）：仅在文件描述符状态发生变化时通知程序，适合需要减少系统调用次数、提高性能的场景。
   - 水平触发（默认）：只要文件描述符处于可读或可写状态，就会持续通知程序，适合需要确保不遗漏任何事件的场景。
   - 通过`addWaitFd`函数中的`edgeTrigger`参数，程序可以选择适合的触发模式。

3. **支持一次性事件（EPOLLONESHOT）**：
   - 如果设置了`isOneshot`参数，那么在事件被触发一次后，文件描述符会自动从epoll实例中移除，避免重复处理同一事件。
   - 这对于某些需要确保事件处理唯一性的场景非常有用。

4. **简化事件处理逻辑**：
   - 使用epoll可以简化事件处理逻辑，程序只需在一个或几个线程中等待epoll事件，然后根据事件类型处理相应的文件描述符，而无需为每个文件描述符创建一个单独的线程或进程。

5. **提高资源利用率**：
   - 由于epoll机制的高效性，程序可以处理更多的并发连接，同时占用更少的系统资源（如CPU、内存等）。

综上所述，将新文件描述符添加到epoll实例中，是网络服务器实现高效并发处理的关键步骤之一。它不仅能够提高I/O事件的处理效率，还能够简化编程模型，降低资源消耗。