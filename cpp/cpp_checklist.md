
### 基础知识
- `const`作用，用法
- `static`作用，用法
-  `inline`作用，用法，编译器是如何处理内联函数的，虚函数可以是内联函数吗？
-  `this`指针的用法
-   什么时候用断言？`assert()`
-  `sizeof`和`strlen`的区别，不同数据结构用`sizeof`的结果会如何（结构体、指针、链表、类等等）
-  C++的`struct`和C中有什么区别，`struct`和`typedef struct`C和C++有什么不同？
-  `union`的作用
-  指针和引用，左值引用和右值引用的区别
-  `using`的用法
-  `explicit`关键字的作用与用法? （什么时候返回对象的函数不调用拷贝构造函数？）
-  `decltype`关键字的作用与用法？
-  少见的概念
    -  `::`运算符的用法
    -  `enum`的用法
    -  知道成员化初始列表的概念吗？`initializer_list`用过没？
    -  `#pargma pack(n)`的使用
    -  位域的作用
    -  关键字`volatile`的使用
    -  `extern "C"`是干嘛的？

### OO
-  如何用C语言来实现C++中的类（大致的思路，封装、多态以及继承的实现）
-  友元类和友元函数`friend`的特性和缺点？
-  谈一下封装吧（`public, protected, friendly, private`的作用域）
-  聊一下对多态的理解（静态多态、动态多态，概念与使用）
-  虚空大军：虚构析函数、纯虚函数、虚函数、虚函数指针、虚函数表、虚继承（含义）
    -  保留字`override`的使用
    -  虚函数与纯虚函数的区别和联系
    -  虚继承与虚函数的相同与不同
    -  模板类、成员模板和虚函数
    -  什么时候用虚析构函数
 -  抽象类、接口类、聚合类 
 
### 内存分配与管理
 -  如何申请到一大块内存（Linux）
 -  `malloc, calloc, realloc, alloca`区别
 -  `malloc free`与`new delete`的用法，例如多维数组
 - 对内存的理解（栈、堆、全局区（静态区），常量区，程序代码区）？内存模型有哪些？
 - 栈和堆的区别，栈和堆的生命周期？堆栈缓存方式的区别？
 - 内存泄露？面对内存泄露或指针越界，有哪些方式？
 - `menset`的用法
 -  扩展问题：
     -  可以`delete this`指针吗？
     -  如何定义一个只能在堆（or 栈）生成对象的类？

### STL
-  常用的容器有哪些操作形式？比如vector，list


### 多线程

### 设计模式
-  用过、看过那些设计模式的代码？手写一个看看？

### 智能指针
 -  `shared_ptr, unique_ptr, weak_ptr`的作用，用法 ？
 
### 类型转换
-  知道有哪些强制类型转换运算符？怎么用的？（`static_cast, dynamic_cast, const_cast, reinterpret_cast, bad_cast`)
-  什么是运行时类型信息(RTTI)? `typeid, type_info`的作用和使用？


### 链接装载库

