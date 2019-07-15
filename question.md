1. 海量数据，查找其中的url，怎么去重，设计合适的数据结构

2. ctrl+c发生什么，怎么通知进程的
3. 进程内存管理静态存储区初始化和未初始化分别怎样存储
4. 父进程和子进程共享那些信息
5. c++的头文件怎么给c项目使用
6. 段错误怎样定位
7. 编译和链接的区别
8. malloc和new的区别
9. 手写string的复制构造函数，拷贝构造函数，带参数构造函数，析构函数
10. tcp怎么确认是否丢包，tcp的SYN序列号怎么生成，select使用了那些额外参数配置
11. tcp服务器断电后，重新来电，客户端会接受什么
12. 智能指针，weak_ptr怎么实现
13. vector的size能否减少
14. 死锁，怎么预防，写出产生死锁的图解
15. 线程加锁后，里面的变量能否访问
16. strcmp/strcpy实现原理
17. weak_ptr在多线程中的应用
18. QT信号槽原理知道吗，如果要自己实现一个你会怎么做？
19. C++11 的原子操作，C++11多线程内存模型
20. std::thread 和操作系统级别的线程有什么区别
21. 文件里很多银行账号 ，统计出现次数最多的几个账号
22. 怎么探查网络数据带宽，在不占满的情况下
23. 什么是中断
24. 进程切换怎么实现 怎么保留断点信息。
25. 哈希函数及哈希冲突
26. 日志文件10亿条，每条记录 用户账号 上线时间 下线时间。时间从0---86400s，最小时间复杂度和最小空间复杂度  得出每一秒在线人数的统计情况
27. ARP欺诈
28. 一个进程里包含什么 
29. 一个进程在32系统上寻址空间最多多大
30. 假如有一万个单词，分别用空格隔开，统计重复单词的次数
31. const函数和非const函数可以实现重载吗
32. 基类里private成员函数可以声明为虚函数吗
33. vector使用迭代器进行循环，当循环开始时push_back一个数在后面，会发生什么
34. 假如有一万个单词，分别用空格隔开，统计重复单词的次数
35. 讲一下MTU和MSS
36. 进程A和进程B只能进行网络通信，当进程B挂掉之后进程A如何快速知道进程B挂掉
37. vector v1(100,0) 和 vector v2(200,0) ，它们sizeof大小分别为多少
38. strcpy使用可能有什么缺陷
39. 子类继承父类，如果父类的析构函数不是虚函数，会有什么问题？
40. extern c有什么用
41. rand5实现rand7  有一个能可以生成1-6的随机数生成器，实现一个随机1-12的随机数生成器。
42. static函数可以修改成员变量
43. 实现atoi
44. 手写简单的一个线程 手写多个线程按顺序执行 手写一定区间的随机数
45. 写一个memcpy、strcpy
46. 给几百万个网址，如何高效找出特定网址是否在其中?(布隆过滤器) 布隆过滤器优缺点，如何解决其缺点？
47. 50个红球50个蓝球，放到2个袋子里，从两个袋子各取1个球，让2个都是红球的概率最大，怎么放
48. Linux下删除同一文件夹下所有满足条件的文件
49. new/delete和malloc/free的区别
50. vector的结构？vector拷贝时发生什么
51. 一个数组，只有一个数字出现奇数次，其余数字出现偶数次，如何得到这个数字？如果出现奇数次的数字有2个呢？
52. cmake和makefile的区别
53. 2g物理内存，new一个3g的数组时发生什么？
54. char (\*p) [] 、char \*p[]、char (\*p)()的区别？
55. 说说select、poll、epoll区别？
56. 熟悉句柄么？程序执行后句柄如何处理，如何修改可打开句柄数量？
57. 如何设计一个高并发的分布式服务器？
58. 1G内存，4G url，求重复的url
59. 迭代器的++it和it++哪个好
	1. 前++返回引用，后++返回一个临时对象
60. 拷贝构造函数的参数为什么要传引用而不是传值？
61. strncpy的实现？为什么strcpy是不安全的？可以结合缓冲区溢出说一下。
62. STL的map底层是怎么样的？
63. C++的lambda表达式怎么用？按值捕获和按引用捕获讲一下
64. sizeof空类的大小？为什么
65. 静态链接库和动态链接库的区别
66. 如何防止c++头文件被重复引用
67. 什么叫软链接和硬链接，他们的区别是什么
68. io复用和异步io有什么区别？ 	
69. 多线程、多进程在实际服务器中的应用
70. 讲讲自旋锁，递归锁，乐观锁，悲观锁
71. 面向对象有什么优点？为什么要有面向对象？
72. 存在大量closed_wait有什么危害
73. LRU
74. malloc内存分配机制是怎么样的,在哪里分配内存，最大可以申请多大的内存
75. 怎么用C实现一些C++的特性吗，比如重载、多态？
76. 进程调度算法
77. C++智能指针如何解决内存泄露问题
78. 各种语言的应用场景，如python的应用场景
79. 用c++写一个简单的多态实现。
80. 判断一个字符串表示的ip地址是否合法
81. 如何检测死循环，答perf工具去看时间，说不用这个工具呢，说思路。
82. 为什么要字节对齐
83. 高并发频繁加锁对性能有影响吗，怎么优化
84. http报文特别大，每次读多少字节，一个报文分几次发送，怎么完整接受，有没有试过发送特别大的报文，这是TCP里面的什么问题（粘包）
85. 服务器怎么实现的
86. 子线程如果卡住了，会一直等待吗
87. 怎么判断哪个线程空闲，怎么让程序充分利用多核，线程实际上是在多核上跑吗
88. new背后会发生什么
89. 在类里面给成员赋值，会在什么时候初始化
90. 为什么析构函数不要抛出异常，构造函数中出现异常会怎么样
91. lamda表达式原理，lamda表达式内的变量的生命周期   编译器是如何实现lambda表达式
92. http2.0和1.1区别，多路复用原理
93. 介绍下重载和重写，f(int, float), f(float, int), 调用f(1, 1)用的哪个
94. stl用过哪些，sort怎么实现，unordered_map实现
95. 手写线程安全的单例模式
96. 静态成员函数能调用非静态成员函数吗
97. 编译的过程
98. 多进程下gdb调试流程
99. https 客户端和服务器是如何实现协议选择的，现在常用的协议是什么 https 客户端鉴定服务器的过程
100. 操作系统的段和页式管理，段页式优点？
101. n个数中取m个数，要求每个数取出的概率相等
102. 手写LRU
103. 在点击一个exe文件时，会发生什么样的一个操作
104. LRU有什么缺点
105. 写一个LRU的缓存，需要完成超时淘汰和LRU淘汰。