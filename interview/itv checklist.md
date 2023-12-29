## BIG PICTURE

- leetcode 101
- 基础：cpp, system, concurrency, os, net, db
- 业务：planning, self driving
- 业务：cuda, architecture
- 设计：system design, TDD



JD:

- C/C++, Makefile
- tests case dev, tests automation, failure analysis
- software testing
- CUDA
- VectorCAST, Bullyseye, Gcov, Coverity





## 2. 基础 

### 2.1 cpp 

#### 内存布局

首先关注下cpp的核心问题：

> cpp11 的内存分布是怎样的？编译链接机制、内存布局（memory layout）、对象模型

堆 区 、 栈 区 、 全 局 区 、 文 字 常 量 区 、 程 序 代 码 区 。

ref: https://zhuanlan.zhihu.com/p/380147337

 堆内存和栈内存有什么区别？

关键词：连续、系统自动分配，1M。

如何申请一大片内存（>4G),申请超过内存条容量的内存会发生什么？

malloc libc。

ref: https://www.zhihu.com/question/20836462

关键词：寻址空间、页交换



> 什么是内存对齐？为什么要内存对齐？

struct更快



C 和 C++ 的一些区别，比如 new、delete 和 malloc、free 的区别

>new能自动计算需要分配的内存空间，而malloc需要手工计算字节数。 
>
>new与delete直接带具体类型的指针，malloc和free返回void类型的指针。 
>
>new类型是安全的，而malloc不是。例如int *p = new float[2];就会报错；而int p = m a l l o c ( 2 sizeof(int))编译时编译器就无法指出错误来。
>
> new一般分为两步：new操作和构造。new操作对应与malloc，但new操作可以重载，可以自定义内存分配 策略，不做内存分配，甚至分配到非内存设备上，而malloc不行。 new调用构造函数，malloc不能；delete调用析构函数，而free不能。 
>
>malloc/free需要库文件stdlib.h的支持，new/delete则不需要！
>
> 注 意 ： d e l e t e 和 f r e e 被 调 用 后 ， 内 存 不 会 立 即 回 收 ， 指 针 也 不 会 指 向 空 ， d e l e t e 或 f r e e 仅 仅 是 告 诉 操 作 系 统 ， 这 一 块 内 存 被 释 放 了 ， 可 以 用 作 其 他 用 途 。 但 是 由 于 没 有 重 新 对 这 块 内 存 进 行 写 操 作 ， 所 以 内 存 中 的 变 量 数 值 并 没 有 发 生 变 化 ， 出 现 野 指 针 的 情 况 。 因 此 ， 释 放 完 内 存 后 ， 应 该 讲 该 指 针 指 向 N U L L 。



#### 移动语义

这一部分仔细反复阅读这个：https://changkun.de/modern-cpp/zh-cn/03-runtime/index.html#3-3-%E5%8F%B3%E5%80%BC%E5%BC%95%E7%94%A8

 聊一聊移动语义，左值右值，完美转发的理解。

key: 左值右值的定义(将亡值) -> 左值转右值 -> 左值转发/右值转发 -> 移动语义 -> 完美

右值：表达式结束后不再存在的临时对象。右值转发：移动，续命。



为什么引入右值引用？const 左值可以blind到右值吗？

ref: https://zhuanlan.zhihu.com/p/85432613



std::move实现机制，该过程是否涉及到引用塌陷？

ref: https://wendeng.github.io/2019/05/14/c++%E5%9F%BA%E7%A1%80/c++11std-move%E4%BD%BF%E7%94%A8%E4%B8%8E%E5%8E%9F%E7%90%86/

ref: https://www.cnblogs.com/studywithallofyou/p/14576304.html



完美转发，std::forward的实现机制。



基于c++11实现std::make_unique

关键词：forward, [可变参数模板](https://changkun.de/modern-cpp/zh-cn/02-usability/index.html#%E5%8F%98%E9%95%BF%E5%8F%82%E6%95%B0%E6%A8%A1%E6%9D%BF)



#### 虚函数

> 虚函数的实现，虚函数表/虚基类表。

什么是virtual table

ref: https://zhuanlan.zhihu.com/p/380147337

关键词：vfptr -> vftable , 覆写。



为什么会有这个东西？

ref: https://www.zhihu.com/question/389546003

关键词：struct函数指针，静态绑定static binding, 动态绑定，连续内存+offset.



虚函数的性能问题：

[虚函数真的就那么慢吗？它的开销究竟在哪里？来看这4段代码！ (qq.com)](https://mp.weixin.qq.com/s?__biz=MzkyODE5NjU2Mw==&mid=2247485056&idx=1&sn=5a3e5b93f6d8872aa5fdfeee4e509a9f&chksm=c21d343cf56abd2a6abfaef8caff16dd6fb835bdaf3d4901f55e26eea45a059411c1d1bd94ea&scene=21)

关键词：分支预测，内联优化。



多态： 动态绑定，静态多态

继承、虚继承、菱形继承等



#### 智能指针

> 三种智能指针的差异？如何使用？

ref: https://changkun.de/modern-cpp/zh-cn/05-pointers/index.html

key: 引用计数，强引用/弱引用

> 智能指针原理：引用计数、RAII（资源获取即初始化）思想，几个智能指针的区别。



强引用与弱引用的区别。

关键词：std::week_ptr, expired(), 



ref: https://zhuanlan.zhihu.com/p/54078587







#### 细节问题

> explicit关键字

防止隐式转换。



> constexpr和const的区别

const: 只读、常量 -> constexpr: 常量， 编译期检查



统一的类成员初始化语法与 std::initializer_list<T>



> 注解标签（attributes）



> 一些关键字的作用：static、const、volatile、**extern**



> 四种类型转换：static_cast, dynamic_cast, const_cast, reinterpret_cast

ref: [强制类型转换运算符](https://github.com/huihut/interview#%E5%BC%BA%E5%88%B6%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2%E8%BF%90%E7%AE%97%E7%AC%A6)



位域：



仿函数：

https://blog.csdn.net/codedoctor/article/details/79654690

类+()重载，用lambda更方便。



#### STL与模板

STL和模板也不能漏掉

> STL部分容器的实现原理，如 vector、deque、map（红黑树）、hashmap。

vector:

```c++
template <class _Ty, class _Alloc = allocator<_Ty>>
class vector{
    ...
protected: // 扩容的机制，capacity, reserve为什么更快？
    pointer _Myfirst;
    pointer _Mylast;
    pointer _Myend;
};
```

deque:







如何保持树的平衡?





hash, 一致性hash，Hash 表的原理，冲突处理

ref: https://interviewguide.cn/#/Doc/Knowledge/C++/%E5%9F%BA%E7%A1%80%E8%AF%AD%E6%B3%95/%E5%9F%BA%E7%A1%80%E8%AF%AD%E6%B3%95?id=%e4%bb%80%e4%b9%88%e6%98%af%e4%b8%80%e8%87%b4%e6%80%a7%e5%93%88%e5%b8%8c





模板特化、偏特化，萃取 traits 技巧



c++14/17/20的新特性

c++17 

c++20 concepts/module/coroutine/ranges



#### 高阶问题

首先说三个智能指针的实现以及内存模型（建议使用make shared因为make shared的内存模型可能是control block和对象连在一起的、control block为什么要有weak ref count），unique的T[]偏特化，shared from this的设计思路crtp，智能指针的设计指导思想raii，智能指针的线程安全性，等等。



然后由智能指针的内存模型说到堆栈的分配，虚拟地址，exec加载，动态链接库，mmap，堆的实现（freelist、分配一块大的页（好像是、记不住了）、直接mmap），再展开可以说页表、tlb、tlb刷新，进程切换（switch mm、switch to、系统调用int 过程、调用约定，fork把子进程tss的rax设置为0所以子进程fork返回0），带进程id的tlb，ipc，等等。



T[]偏特化说到C++模板的偏序原则，说到msvc、gcc、clang的array decay的偏序规则不一致，说到C++模板实例化原理（名称查找、参数推导、参数替换、重载决议），SFINAE，模板元编程。



crtp说到多态和system F。爬一遍lambda cube，说到dependent type以及C++阉割版的非类型模板参数。再说到递归，为什么system F里的递归就比stlc里的和谐，强在哪。



cpp多态实现原理。
容器rotate的三种实现。
vector如何动态扩容，stl默认内存池如何实现
手写一个shared_ptr(不考虑各种异常)
仅遍历一次ip字符串，判断该ip格式是否正确
cpp实现向量的延迟计算
cpp定位内存泄露和profile的工具

more to see: https://www.zhihu.com/question/60911582





### 2.2 system (csapp)

什么是虚拟内存？



IO 多路复用是什么？有哪些 api？

> 下面举一个例子，模拟一个tcp服务器处理30个客户socket。
> 假设你是一个老师，让30个学生解答一道题目，然后检查学生做的是否正确，你有下面几个选择：
>
> \1. 第一种选择：**按顺序逐个检查**，先检查A，然后是B，之后是C、D。。。这中间如果有一个学生卡主，全班都会被耽误。
> 这种模式就好比，你用循环挨个处理socket，根本不具有并发能力。
> \2. 第二种选择：你**创建30个分身**，每个分身检查一个学生的答案是否正确。 这种类似于为每一个用户创建一个进程或者线程处理连接。
> \3. 第三种选择，你**站在讲台上等，谁解答完谁举手**。这时C、D举手，表示他们解答问题完毕，你下去依次检查C、D的答案，然后继续回到讲台上等。此时E、A又举手，然后去处理E和A。。。 
> 这种就是IO复用模型，Linux下的select、poll和epoll就是干这个的。将用户socket对应的fd注册进epoll，然后epoll帮你监听哪些socket上有消息到达，这样就避免了大量的无用操作。此时的socket应该采用**非阻塞模式**。
> 这样，整个过程只在调用select、poll、epoll这些调用的时候才会阻塞，收发客户消息是不会阻塞的，整个进程或者线程就被充分利用起来，这就是**事件驱动**，所谓的reactor模式。



select 和 epoll 的区别？水平触发和边缘触发的区别是啥？使用的时候需要注意什么？



epoll 储存描述符的数据结构是什么？



select 有描述符限制吗？是多少？



进程 / 线程 / 协程区别？go 和 swoole 的协程实现有啥区别？（分配资源的基本单位 / 运行和调度的基本单位 / 用户线程，M:N 模型和 N:1 模型）



C++从代码到可执行程序经历了什么？







###  2.3 concurrency

四个线程，严格打印1，2，3，4?

多线程同步和互斥有何异同，在什么情况下分别使用他们？举例说明



### 2.4 OS

#### 2.4.1 并发

自旋锁是什么？有什么缺点？读写锁呢？



线程与进程的区别? (Process control block, scheduler, lightweight v/s heavyweight)



共享内存是如何实现的? (mmap 以及 shmmap 的实现)



你写过操作系统? 你如何实现你的调度算法? (Priority + Round Robin )



你的操作系统怎么管理你的内存? (Page allocator)



你怎么管理你的信号? (Signal Capture, System calls, and etc,.)

### 2.5 networking

做TCP拥塞控制?

三次握手，四次挥手（也不多, 11 个 state, 7 ~ 8 条线, 1~2 小时能背下来.）

## 3 业务: Planning



Hybrid A*, Lattice, Quintic，DWA或其他等规划算法



NMPC、A`*`、D`*`算法



ref: https://www.zhihu.com/column/c_1159478626213863424



## 4 业务：CUDA, Archit

### 4.1 CUDA 

#### 4.1.1 GPU硬件



#### 4.1.2 CUDA

比如block/grid/thread/stream/warp divergence//bank conflict，对GPU访存开销有着清晰的认识和了解。

我们做项目的时候大部分也是在用CUDA Runtime，用Thrust，用Unified Virtual Address Space，

- CUDA 双线性差值原理说一下

- 还知道哪些差值算法（不知道了）

- CUDA stream 相关

- CUDA thread 组织形式，与 SM、SP 关系

- CUDA 共享内存，矩阵乘法

  请简要描述GPU的存储结构。CPU与GPU之间怎样传输数据？试用cuda函数说明。

  - 请完成矩阵转置的cuda实现，并进行优化。
  - 请使用CUDA编写一个数组求和的函数。要求输入为数组首地址及数组长度，输出数组所有元素之和。

  （注：不能使用本地IDE）

  请用CUDA/opencl/pthread多线程/openmp等任意并行化方式实现快速排序算法



- CUDA优化的思路

  review reduce.pdf,整理出来。




**1. [What do you mean by Cuda core?](http://www.comrevo.com/2017/05/what-do-you-mean-by-cuda-core.html)**

SM->SP->Cuda core

**2. [What are the components of gpu?](http://www.comrevo.com/2017/05/what-are-components-of-gpu.html)**



**3. [What are the different memories used in gpu?](http://www.comrevo.com/2017/05/what-are-different-memories-used-in-gpu.html)**

**4. [What are memories which are particular to each thread?](http://www.comrevo.com/2017/05/what-are-memories-which-are-particular-to-each-thread-in-cuda.html)**

**5. [Which memory is used by all threads in a single block?](http://www.comrevo.com/2017/05/what-are-memories-which-are-particular-to-each-thread-in-cuda.html)**

**6. [What is the use of Constant memory?](http://www.comrevo.com/2017/05/what-is-use-of-constant-memory-in-cuda.html)**

**7. [What is the use of Texture memory?](http://www.comrevo.com/2017/05/what-is-use-of-texture-memory-in-cuda.html)**

**8. [What do you mean by bock and grid in Cuda?](http://www.comrevo.com/2017/05/what-do-you-mean-by-block-and-grid-in-cuda.html)**

**9. [What is the difference between CPU and GPU?](http://www.comrevo.com/2017/05/what-is-difference-between-cpu-and-gpu.html)**

**10. [Why do we need GPU when already we have CPU?](http://www.comrevo.com/2017/05/why-do-we-need-gpu-when-already-we-have-cpu.html)**

**11. [What is the advantage of shared memory in CUDA?](http://www.comrevo.com/2017/05/what-is-the-advantage-of-shared-memory-in-cuda.html)**

**12. [What is the advantage of constant memory in CUDA?](http://www.comrevo.com/2017/05/what-is-advantage-of-constant-memory-in-cuda.html)**

**13. [What is warp in CUDA?](http://www.comrevo.com/2017/05/what-is-warp-in-cuda.html)**

**14. [What is the use of cudaMalloc() function and what arguments does it accept?](http://www.comrevo.com/2017/06/what-is-use-of-cudaMalloc-function-and-what-arguments-does-it-accept.html)**

**15. [What is the use of cudaMemcpy() function and what arguments does it accept?](http://www.comrevo.com/2017/06/what-is-use-of-cudamemcpy-function-and-what-arguments-does-it-accept.html)**

**16. [What is the use of cudaFree() function and what arguments does it accept?](http://www.comrevo.com/2017/06/what-is-use-of-cudafree-function-and-what-arguments-does-it-accept.html)**

**17. [What is kernel in Cuda?](http://www.comrevo.com/2017/06/what-is-kernel-in-cuda.html)**

**18. [How kernel is defined in Cuda?](http://www.comrevo.com/2017/06/how-kernel-is-defined-in-cuda.html)**

**19. [How to define a kernel which is called from another kernel?](http://www.comrevo.com/2017/06/how-to-define-kernel-which-is-called-from-another-kernel.html)**

**20. [How kernels are called from main() function?](http://www.comrevo.com/2017/06/how-kernels-are-called-from-main-function.html)**

**21. [What is dim3 in Cuda?](http://www.comrevo.com/2017/06/what-is-dim3-in-cuda.html)**

**22. [How can we define a multi dimensional structure of grid](http://www.comrevo.com/2017/06/how-can-we-define-multi-dimensional-structure-of-grid.html)?**

**23. [How can we define a multi dimensional structure of block?](http://www.comrevo.com/2017/06/how-can-we-define-multi-dimensional-structure-of-block.html)**

**24. [What are the keywords used for finding block id and thread id?](http://www.comrevo.com/2017/06/what-are-keywords-used-for-finding-block-id-and-thread-id-in-cuda.html)**

**25. [What are keywords used for block size and grid size?](http://www.comrevo.com/2017/06/what-are-keywords-used-for-block-size-and-grid-size.html)**

**26. [Name any Nvidia gpu card which consists of two gpus?](http://www.comrevo.com/2017/06/name-any-nvidia-gpu-card-which-consists-of-two-gpus.html)**

**27. [How to find out number of gpus in your system?](http://www.comrevo.com/2017/06/how-to-find-out-number-of-gpus-in-your-system.html)**

**28. [How to find out id of current gpu on your system?](http://www.comrevo.com/2017/06/how-to-find-out-id-of-current-gpu-in-cuda.html)**

**29. [What are the different properties of gpu?](http://www.comrevo.com/2017/06/what-are-different-properties-of-gpu.html)**

**30. [How to find out properties of gpu?](http://www.comrevo.com/2017/06/how-to-find-out-properties-of-gpu.html)**

**31. Which properties of gpu are used to find its compute capability?**

**32. How to set any gpu as current gpu when its id is given?**

**33. What is the use of cudaChooseDevice() function and what arguments does it accept?**

**34. What is the use of memset() function and what arguments does it accept?**

**35. How to declare a variable which is to be saved in shared memory?**

**36. How to declare a variable which is to be saved in constant memory?**

**37. What is the use of cudaMemcpyToSymbol() function and what arguments does it accept?**

**38. What are the different alternatives to Cuda?**

~~**39. What is parallel computing?**~~

~~**40. What are the advantages of parallel computing?**~~

**41. What are the different architectures according to Flynn's taxonaomy?**

**42. What are different parallel programming models?**

**43. What are the two basic classes of parallel architectures?**

**44. Which are general purpose parallel architectures?**

**45. Which are special purpose parallel architecures?**

**46. What is the unit used to represent frequency of a gpu?**

**47. What do you understand by Mflop?**

**48. What do you mean by speedup with respect to performance of parallel computers?**

**49. What is Amdahl's law? Give the expression for the speed up.**



- What is the first thing (or few things) you should optimize for when writing CUDA code?
- Describe the different memories (global, shared, constant, register) and their attributes.
- What are warps, threads, thread blocks, and grids? Describe their relationships/hierarchy.
- What happens when threads in the same warp take different control paths? Threads in different warps?
- How are trigonometric functions potentially different on CUDA?



- You have the same set of load and store operations, from global memory and to either shared or global memory (I would have a separate problem for each). Compare two different sequences of those same instructions, e.g. ld a -> st a -> ld b -> st b, versus ld a -> ld b -> st a -> st b, where a and b are the registers involved in the operations. Describe the situations in which one sequence is superior to the other.



#### 4.1.3 Data Parallel



### 4.2 Architect

cache, 流水线。



## 5. Design



##  

### REF

1. https://github.com/huihut/interview
2. https://www.cnblogs.com/qg-whz/p/4909359.html





## 6. Folly



### 6.1 ConcurrentHashMap

> Readers are always wait-free, writer are sharded（分片）, but take a lock

特性：

除了无锁hash map，多线程下性能比任何并行hash map好。但无锁必须保证原子性，且不支持erase操作。





### Testing

