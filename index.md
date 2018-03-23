1.进程线程了解吗？

2.线程里面有什么是独立的？

3.一个进程一定要有一个线程吗？没有线程的进程是什么？

4.协程是什么？

5.同步和互斥是怎么做的？

6.线程间的同步和互斥是怎么做的？

7.守护进程和僵尸进程，孤儿进程有什么了解？

8.系统出现僵尸进程，为什么产生和怎么解决？

9.软连接和硬连接了解吗？

10.硬连接和软连接删了，原对象会如何？

11.硬连接和软连接的底层原理？

12.Inode是什么？

13.强类型和弱类型，静态类型动态类型是什么？

(14) TCP/UDP的了解？

(15) Tcp和udp的使用场景

(16) Tcp粘包

(17) Tcp的time_wait (到这里我觉得面试官面不下去了)

(18) http1.0和1.1有什么区别

(19) https协议？原理？端到端中间的过程。

(20) 对称加密和非对称加密？

(21) Cookies和session的关系

(22) Cookies的最大保存时间

(23) Mysql索引的原理，底层是怎么存的？

(24) 主键和唯一键有什么区别？

(25) Varchar和char的区别？

(26) UTF-8下面varchar能占多少字符？GBK呢？

(27) 说下你知道的排序，比较一下他们的优缺点，复杂度和应用场景。

3、 用过什么服务器，能讲述下一个请求到来到处理完全部的流程。

4、 服务器如何解决大量的用户访问。我说线程池。说到等待队列。他说等待队列的大小值如何给定。 服务器如何确定访问的最大 客户端 数目。

5、 HTTP 协议讲下， HTTPS 如何加密， HTTPS 加密算法 SSL。HTTP GET POST区别。 HTTP请求会保持连接吗？

10、 使用过 NoSQL 吗？大概讲了下。LINUX使用的多不？在LINUX下做过什么事情。。我说搭过Hadoop

2.        进程间通信方式

管道（pipe）
信号（signal）
Socket
13.    十亿qq号无顺序，有N个号码 256MB内存限制，设计算法快速找到

2.time_wait是什么，产生在 tcp连接那一端，为什么要有，如果没有的话 什么危害：答案估计大家都背的烂透了 我就不BB了
3.time_wait时间大概是多少：具体时间是4-6分钟，也就是2个报文最长生存时间。
4.滑动窗口协议 的概念。
5，阻塞控制中，阻塞窗口大小怎么动态变化的：具体的分两种情况说就行：一个是慢启动 一个是快重传，大家看下课本，这个要画图文字说不清
 
静态变量和全局变量的区别：The difference is that i has internal linkage, and j has external linkage. This means you can access j from other files that you link with, whereas i is only available in the file where it is declared.
TCP洪水攻击
JVM jdk jre
tcp 断开连接过程
tcp首部结构
源端口 source port
目的端口 destination port,序号 sequence number, 确认号 acknowledgment number,  窗口大小 window size, 检验和 checksum

网络字节序，如何判断机器大小端模式，自己实现主机跟网络字节的转换的函数
网络七层协议，描述一个http从发送请求到接收消息整个七层过程，用到的协议
设计类似于LRU算法的一个固定cache内存交换算法，要求get,set,delete,高效，重点是数据结构的选用，后来面试官说最好用hash表
 
设计一个类似搜索扣扣好友列表，例如输入a显示前缀为a的所有好友，我说对好友列表用字典排序，然后面试官说如果给搜索字段很长的话性能不好，然后我说了用文件索引，然后查找索引，面试官让说出来具体实现
.Mysql最左前缀原则
https://www.nowcoder.com/discuss/59643?type=2&order=0&pos=106&page=1
