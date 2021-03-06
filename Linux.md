
**1. 一个程序从开始运行到结束的完整过程。** 

	A: 1. 预编译: 生成一个.i文件。

	   2. 编译: 对预处理后的文件进行语法分析，词法分析，语义分析，符号汇总，然后生成汇编代码。

	   3. 汇编: 将汇编代码转成二进制文件，二进制文件就可以让机器来读取。每一条汇编语句都会产生一句机器语言

	   4. 链接: 把Lib文件和.obj文件合起來生成EXE文件

<img class="inline" src="https://raw.githubusercontent.com/cloi1994/TC/master/1338355-20180304130814151-249194330.png" width="60%">

**2. linux里ipc有哪些?**
	
	A: 信号(Signal)，信号量(Semaphore)，消息(Message Q)，

	共享内存(shared memory)，文件系统(FS). Socket, 管道(PIPE)

**3. 为啥要cache呢？** 

	A: 内存的读写速度跟不上cpu所以加了个cache。

	   作用是加快CPU访问常见数据的速度，cache做的事情是把内存里面常用的存储数据存在自己这里供CPU读取，

	   因为cache的访问延迟远远小于内存，所以访问这部分存在cache里的数据就会比直接去访问内存快的多，大概快一个量级。
 
**4. 数据写到cache里以后，具体是怎样更新到内存中的?**

	A:　　写入数据时： 
	
	      第一步，CPU将数据写入Cache
	
	      第二步，将Cache数据传送到Memory中相应的位置 
	
	      读取数据时： 
	
	      第一步，将Memory中的数据传送到Cache中
	
	      第二步，CPU从Cache中读取数据
	  
**5. Cache的写操作有透写（Write-Through）和回写（Write-Back）两种方式**

	A: 1. 在透写式Cache中，CPU的数据总是写入到内存中，如果对应内存位置的数据在Cache中有一个备份，
	那么这个备份也要更新，保证内存和Cache中的数据永远同步。
	
	   2. 在回写式Cache中，把要写的数据只写到Cache中，并对Cache对应的位置做一个标记，
	只在必要的时候才会将数据更新到内存中。

	   3. 透写方式存在性能瓶颈，性能低于回写方式，现在的CPU设计基本上都是采用Cache回写方式。


**6. 同步与异步?**

	A: 同步: 当一个同步调用发出去后，调用者要一直等待调用结果的通知后，才能进行后续的执行。 
	
	   异步：当一个异步调用发出去后，调用者不能立即得到调用结果的返回。
	   
	   生活中的例子:
	   
	   同步买奶茶：小明点单交钱，然后等着拿奶茶
	   
	   异步买奶茶：小明点单交钱，店员给小明一个小票，等小明奶茶做好了，再来取。

**7. 阻塞与非阻塞?**

	A: 阻塞调用在发出去后，在消息返回之前，当前进/线程会被挂起，直到有消息返回，当前进/线程才会被激活.

	   非阻塞调用在发出去后，不会阻塞当前进/线程，而会立即返回。

	   生活中的例子:

	   阻塞买奶茶：小明点单交钱，干等着拿奶茶，什么事都不做；

	   非阻塞买奶茶：小明点单交钱，等着拿奶茶，等的过程中，时不时刷刷微博、朋友圈。
	   
**8. I/O 多路复用的作用是什么**

	A: 关于I/O多路复用(又被称为“事件驱动”)，首先要理解的是，操作系统为你提供了一个功能，
	   
	   当你的某个socket可读或者可写的时候，它可以给你一个通知。这样当配合非阻塞的socket使用时，
	   
	   只有当系统通知我哪个描述符可读了，我才去执行read操作，可以保证每次read都能读到有效数据而不做纯返回-1和EAGAIN的无用功。
	   
	   注: 当用户进程调用了select，poll, epoll 那么整个进程会被block, fd/socket IO不block

**9. Epoll与Select区别以及epoll优点，为什么一般情况下epoll性能比select好?**

	A: Select: select 函数监视的文件描述符分3类，分别是writefds、readfds、和exceptfds。
	           调用后select函数会阻塞，直到有描述副就绪（有数据 可读、可写、或者有except），
	           或者超时（timeout指定等待时间，如果立即返回设为null即可），
	           函数返回。当select函数返回后，可以 通过遍历fdset，来找到就绪的描述符。
	   
	           select的一个缺点在于单个进程能够监视的文件描述符的数量存在最大限制，在Linux上一般为1024
	   
	   Poll: pollfd结构包含了要监视的event和发生的event，不再使用select“参数-值”传递的方式。
	         同时，pollfd并没有最大数量限制（但是数量过大后性能也是会下降）。 
		 和select函数一样，poll返回后，需要轮询pollfd来获取就绪的描述符。
		 
		 select和poll都需要在返回后，通过遍历文件描述符来获取已经就绪的socket。
		 事实上，同时连接的大量客户端在一时刻可能只有很少的处于就绪状态，
		 因此随着监视的描述符数量的增长，其效率也会线性下降。

	   Epoll: 当连接有I/O流事件产生的时候，epoll就会去告诉进程哪个连接有I/O流事件产生，
	   
	          因为epoll事先通过epoll_ctl()来注册一个文件描述符(fd)，一旦基于某个文件描述符就绪时，
		  内核会采用类似callback的回调机制，迅速激活这个文件描述符，当进程调用epoll_wait() 时便得到通知。
		  (此处去掉了遍历文件描述符，而是通过监听回调的的机制。这正是epoll的魅力所在。)

	   Epoll 优点: 可以说是I/O多路复用最新的一个实现，epoll 修复了poll 和select绝大部分问题, 比如：

		       Epoll 现在是线程安全的。 
		       
		       监视的描述符数量不受限制 > 2048
		       
		       IO的效率不会随着监视fd的数量的增长而下降
		       
		       epoll不同于select和poll轮询的方式，而是通过每个fd定义的回调函数来实现的。只有就绪的fd才会执行回调函数。
		       		       
	   Nginx: 异步，非阻塞，IO多路复用

**9. ET模式与LT模式**

	A: Level Triggered ( LT )水平触发，默认方式，即当epoll_wait检测到描述符(fd)事件发生并将此事件通知应用程序，
	应用程序可以不立即处理该事件。，因为只要对应的文件描述符中还有数据未读取或者处于可写状态，都会有通知。
	下次调用epoll_wait时，会再次响应应用程序并通知此事件。并且同时支持block和no-block socket
	
	   Block-LT - 每次只读/写1个字符, 只样确保不阻塞其他 fd/socket IO. (epoll_wait 只trigger 可读/可写)

	   Edge Triggered ( ET )边缘触发，即当epoll_wait检测到描述符事件发生并将此事件通知应用程序，
	应用程序必须立即处理该事件。如果不处理，例如，在read的时候没有一次性把缓冲区的数据全部读出来，
	下次调用epoll_wait时，不会再次响应应用程序并通知此事件。除非有新的事件发生，不然监听的描述符是无法被触发。
	只能在非阻塞的套接字(socket)上使用。
	
	在一个非阻塞的socket上调用read/write函数, 返回的errno为EAGAIN或者EWOULDBLOCK 
	这个错误表示资源暂时不够，read时，读缓冲区没有数据，或者write时，写缓冲区满了。
	遇到这种情况，如果是阻塞socket，read/write就要阻塞掉。而如果是非阻塞socket，read/write立即返回-1， 
	同时errno设置为EAGAIN。

	注：ET模式在很大程度上减少了epoll事件被重复触发的次数，因此效率要比LT模式高。
	epoll工作在ET模式的时候，必须使用非阻塞套接口，以避免由于一个文件句柄的阻塞读/阻塞写操作把处理多个文件描述符的任务饿死。

**10. ET模式下accept存在的问题 考虑这种情况：多个连接同时到达，**

	A: ET模式下accept存在的问题 考虑这种情况：多个连接同时到达，
	服务器的TCP就绪队列瞬间积累多个就绪连接，由于是边缘触发模式，epoll只会通知一次，
	accept只处理一个连接，导致TCP就绪队列中剩下的连接都得不到处理。
	解决办法是用while循环抱住accept调用，处理完TCP就绪队列中的所有连接后再退出循环。
	如何知道是否处理完就绪队列中的所有连接呢？accept返回-1并且errno设置为EAGAIN就表示所有连接都处理完。
	
        while ((conn_sock = accept(listenfd,(struct sockaddr *) &remote, (size_t *)&addrlen


**10. Epoll ET下非阻塞读，为什么不能是阻塞**

	A: 即当epoll_wait检测到描述符事件发生并将此事件通知应用程序，
	   序必须立即处理该事件。如果不处理，例如，在read的时候没有一次性把缓冲区的数据全部读出来，
	   用epoll_wait时，不会再次响应应用程序并通知此事件。除非有新的事件发生，不然监听的描述符是无法被触发。
	   所以在ET模式的时候，必须使用while循环抱住accept用, 可是如果使用了while循环可能会导致阻塞读/阻塞写而把其他多个文件描述符
	   (fd)的任务饿死。

**12. linux统计文本中每行第二个字段的和（awk搞定)**

	A: cat a.txt | awk '{ x + $2}'
   
**13. KVM和Docker的区别是什么**

	A: 1. DOCKER采用的是容器虚拟化，kVM是硬件虚拟化，
	
	      Docker 的基础是 Linux 容器（LXC）等技术。在 LXC 的基础上 Docker 进行了进一步的封装，	   
	      
	      让用户不需要去关心容器的管理，使得操作更为简便。用户操作 Docker 的容器就像操作一个快速轻量级的虚拟机一样简单。
	      
	  2. DOCKER充分利用了资源，更能针对PAAS去布署，KVM现在应用在IAAS方面比较多
	     
	     但是KVM是不可能与DOCKER结合，因为它是从零开始，完成整个硬件设备的虚拟化。
	     
	     用户在资源上的随心所欲，但是面向开发环境的复杂性，它不能降低。
   
**14. 操作系统内存分配?**
 
	A: 伙伴系统 - 目的：最大限度的降低内存的碎片化。 
	
           1. 把内存块分成不同的组(1,2,4,8,16,32....)；
	   
           2. 分配内存时找到能够满足条件的最小的块；如果找不到，就找大的块，然后一分为2，分配一块，留一块；
	   
           3. 回收时：如果有相邻的同样大小的块，则合并

**15. 32位64位计算机区别**

	A:  32位最多能使用4G (3G).
	     
	    64位系统最显著的优点，它可以使用超过4GB的内存 (128G).

**16. 32位计算机，程序能分配的最大内存?**

	A: 32位的程序寻址空间是 4G，因此能用的内存有4G，除掉一些系统等使用的东西，3G左右, 可是結果只有2G (1.6). 

**17. 两个1T文件使用4G内存比较相似度**
