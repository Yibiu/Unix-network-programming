# 第六章 IO复用：select和poll函数

[TOC]



## 一：IO模型

- 阻塞IO模型；
- 非阻塞IO模型；
- IO复用模型；
- 信号驱动IO模型；
- 异步IO模型。





## 二：select函数

### 2.1 select函数

```c++
#include <sys/select.h>
#include <sys/time.h>

int select(int maxfdp1, fd_set *readset, fd_set *writeset, fd_set *exceptset,
	const struct timeval *timeout);

struct timeval {
	long tv_sec;
	long tv_usec;
};
```

timeout参数有三种可能：

- 永远等下去：仅在有一个描述符准备好IO时才返回，此时设置为NULL；
- 等待一段固定时间：有描述符准备好IO时返回或超时返回；
- 不等待：检查描述符后立即返回，即轮询。

### 2.2 select操作

```c++
void FD_ZERO(fd_set *fdset);		// clear all bits in fdset
void FD_SET(int fd, fd_set *fdset);	// turn on the bit for fd in fdset
void FD_CLR(int fd, fd_set *fdset);	// turn off the bit for fd in fdset
int FD_ISSET(int fd, fd_set *fdset);// is the bit for fd on in fdset
```





## 三：poll函数

```c++
#include <poll.h>

int poll(struct pollfd *fdarray, unsigned long nfds, int timeout);

struct pollfd {
	int fd;			// descriptor to check
  	short events;	// events of interest on fd
	short revents;	// events that occurred on fd
};
```

reference to epoll !!!

