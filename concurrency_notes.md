## Book Notes

std::API使用不重要，重点关注：

- 并发的问题（特别是经典问题），及其解决办法

  

### 2. 线程管理

**overview**

basic: 启动, 等待, 后台运行

method: 传参, 转移所有权(这个不太好理解), 动态调整线程数量, tag



#### 2.1 启动，等待 & detach

std::thread 只认函数对象，但可以使用列表初始化或lambda来规避这一问题。

```c++
std::thread my_thread(a_func()); // 错误用法

std::thread my_thread((a_func()));  // 1 多组括号
std::thread my_thread{a_func()};    // 2 列表初始化
std::thread my_thread([]{           // 3 lambda
  a_func();
  b_func();
});
```



注意detach后，线程结束的变量，避免前台进程(或初始化的函数）已经结束，后台进程还在使用传入的局部引用或指针这种case。



join()只是单纯的等待。使用RAII手法对thread进行包装，在其析构函数中进行.join()



后台进程的case:

检测到死锁，需要计算碰撞等信息，但为了不影响主loop的运作，可以使用分离线程将计算过程放在后台进行。

```c++
bool deadlock_handler()
{
  std::vector<std::string> deadlock_agents;
  if(detect_deadlock(deadlock_agents))
  {
    std::thread deadlock_resolver(resolve_deadlock, deadlock_agents, this);
    deadlock_resolver.detach();
  }
  else
    return false
  return true;
}
```



#### 2.2 传参， 转移所有权

传参注意下拷贝语义 & 移动语义即可。

转移所有权类似于`std::unique_ptr`, 使用`std::move`进行转移操作。至于c++20正在讨论的`jthread`就是添加了`nonexcept`声明+noncopyable+析构join而已。当然多个线程还是通过量产来管理:

```c++
std::vector<std::thread> threads;
for(unsigned i=0; i<20; ++i)
{
  threads.push_back(std::thread(do_it, i));
}
std::for_each(threads.begin(), threads.end(), std::mem_fn(&std::thread::join));
```

std::mem_fn是threads[i]对象的成员函数的wrapper而已，ref: [C++11新特性之std::mem_fn函数](https://blog.csdn.net/ANDY_YUN/article/details/106162551)



#### 2.3 运行时决定线程数量 & tag.id

书中这里有个std::parallel_accumulate，当无法获取硬件的线程上限时，可以使用

```c++
unsigned long const hardware_threads = std::thread::hardware_concurrency();
```

来决定。这个CPU核芯的数量一般为4~16之间，比如8代i7低压版为8。



线程id, 下面一段代码讲的很清楚了:

```c++
std::thread::id master_thread; 
void some_core_part_of_algorithm() 
{
  if(std::this_thread::get_id()==master_thread) // 主线程
  {
    do_master_thread_work();
  }
  do_common_work();  // 通用部分
} 
```



### 3. 线程间共享数据

**overview**

issue: 条件竞争，死锁

protection: 互斥量，锁



#### 3.1 数据共享的问题

条件竞争(race condition): 多个线程之间对共享数据进行修改操作。

处理思路：1. 无锁编程；2. 类似于数据库，以事务的方式处理数据更新。



#### 3.2 互斥量

在对共享数据操作之前，加上

```c++
std::muxtex a_mutex;

void oper()
{
  std::lock_guard<std::mutex> guard(a_mutex)；
  // your operation for shared data
}
```

来互斥。当然，指针 或 引用可以很容易的穿过这次lock。

栈的弹出操作时可能发送条件竞争：

```c++
stack<int> s; 
if (! s.empty())
{   
  int const value = s.top();
  s.pop();
  do_something(value); 
}
```

在empty()和top()之前可能其他线程执行了pop()操作, 后造成异常。



method 1:  pop传入一个引用，如果这个引用不存在就pop个空的

```c++
s.pop(value); // 恩，比较麻烦的接口
```



method 2: 无异常抛出的拷贝构造函数或移动构造函数 

就是pop一个不存在的对象时不抛出异常，这个需要开启编译选项，为了这个case搞覆盖面如此之广的操作...(⊙o⊙)…



method 3：返回指向弹出值的指针 (make_shared是个不错的方案，避免new 开销)

对于int这种而言，pop中make_shared的开销比复制一个还大。



so... less api is better

> 简化接口更有利于数据控制，可以保证互斥量将一个操作完全锁住。



#### 3.3 死锁 

一个操作需要两个或两个以上的互斥量时，可能发生两个线程互相等待，从而什么都没做。 

**解决方案和建议：**

- 使用`std::lock` （or std::)锁住两个互斥量防止其相互死锁
- 





### 4. 同步并发操作









## interview
一．概念性问答题
第一题：线程的基本概念、线程的基本状态及状态之间的关系？

 

第二题：线程与进程的区别？

       这个题目问到的概率相当大，计算机专业考研中也常常考到。要想全部答出比较难。

 

第三题：多线程有几种实现方法，都是什么？

 

第四题：多线程同步和互斥有几种实现方法，都是什么？

       我在参加2011年迅雷校园招聘时的一面和二面都被问到这个题目，回答的好将会给面试成绩加不少分。

 

第五题：多线程同步和互斥有何异同，在什么情况下分别使用他们？举例说明。

 

二．选择题
第一题（百度笔试题）：

以下多线程对int型变量x的操作，哪几个不需要进行同步：
A. x=y;      B. x++;    C. ++x;    D. x=1;

 

第二题（阿里巴巴笔试题）

多线程中栈与堆是公有的还是私有的

A：栈公有, 堆私有

B：栈公有，堆公有

C：栈私有, 堆公有

D：栈私有，堆私有

 

三．综合题
第一题（台湾某杀毒软件公司面试题）：

在Windows编程中互斥量与临界区比较类似，请分析一下二者的主要区别。

 

第二题：

一个全局变量tally，两个线程并发执行（代码段都是ThreadProc)，问两个线程都结束后，tally取值范围。

inttally = 0;//glable

voidThreadProc()

{

       for(inti = 1; i <= 50; i++)

              tally += 1;

}

 

第三题（某培训机构的练习题）：

子线程循环 10 次，接着主线程循环 100 次，接着又回到子线程循环 10 次，接着再回到主线程又循环 100 次，如此循环50次，试写出代码。

 

第四题（迅雷笔试题）：

编写一个程序，开启3个线程，这3个线程的ID分别为A、B、C，每个线程将自己的ID在屏幕上打印10遍，要求输出结果必须按ABC的顺序显示；如：ABCABC….依次递推。

 

第五题（Google面试题）

有四个线程1、2、3、4。线程1的功能就是输出1，线程2的功能就是输出2，以此类推.........现在有四个文件ABCD。初始都为空。现要让四个文件呈如下格式：

A：1 2 3 4 1 2....

B：2 3 4 1 2 3....

C：3 4 1 2 3 4....

D：4 1 2 3 4 1....

请设计程序。

 

下面的第六题与第七题也是在考研中或是程序员和软件设计师认证考试中的热门试题。

第六题

生产者消费者问题

这是一个非常经典的多线程题目，题目大意如下：有一个生产者在生产产品，这些产品将提供给若干个消费者去消费，为了使生产者和消费者能并发执行，在两者之间设置一个有多个缓冲区的缓冲池，生产者将它生产的产品放入一个缓冲区中，消费者可以从缓冲区中取走产品进行消费，所有生产者和消费者都是异步方式运行的，但它们必须保持同步，即不允许消费者到一个空的缓冲区中取产品，也不允许生产者向一个已经装满产品且尚未被取走的缓冲区中投放产品。

 

第七题

读者写者问题

这也是一个非常经典的多线程题目，题目大意如下：有一个写者很多读者，多个读者可以同时读文件，但写者在写文件时不允许有读者在读文件，同样有读者读时写者也不能写。



leetcode concurrency problems









