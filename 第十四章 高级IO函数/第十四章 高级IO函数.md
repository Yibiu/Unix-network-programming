# 第十四章 高级IO函数

[TOC]



## 一：套接字超时

设置套接字超时方法：

- 调用alarm，超时时产生 SIGALRM 信号；
- select 非阻塞IO等待；
- 使用 SO_RCVTIMEO 和 SO_SNDTIMEO 设置套接字超时属性。





##二：IO操作函数

###2.1 recv和send

```c++
#include <sys/socket.h>

ssize_t recv(int sockfd, void *buff, size_t nbytes, int flags);
ssize_t send(int sockfd, const void *buff, size_t nbytes, int flags);
```

这两个函数类似 `read` 和 `write` ，不过需要一些额外的参数。

### 2.2 readv和writev

```c++
#include <sys/uio.h>

ssize_t readv(int filedes, const struct iovec *iov, int iovcnt);
ssize_t writev(int filedes, const struct iovec *iov, int iovcnt);
```

这两个函数应用于多个缓冲区的读写。

### 2.3 recvmsg和sendmsg

```c++
#include <sys/socket.h>

ssize_t recvmsg(int sockfd, struct msghdr *msg, int flags);
ssize_t sendmsg(int sockfd, struct msghdr *msg, int flags);

struct msghdr {
	void *msg_name;				// protocol address
  	socklen_t msg_namelen;		// size of protocol address
  	struct iovec *msg_iov;		// scatter/gather array
  	int msg_iovlen;				// #elements in msg_iov
  	void *msg_control;			// ancillary data(cmsghdr struct)
  	socklen_t msg_controllen;	// length of ancillary data
  	int msg_flags;				// flags returned by recvmsg()
};
```

这两个函数是最通用的IO函数，实际上可以把所以read、readv、recv和recvfrom调用换成recvmsg调用；各种输出函数调用也可以替换成sendmsg调用。





## 三：高级轮询

略