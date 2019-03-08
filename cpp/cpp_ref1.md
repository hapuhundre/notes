C++面试常问问题汇总

原文链接：https://blog.csdn.net/qq_25680531/article/details/83067870



1. extern关键字的作用

     extern置于变量或函数前，用于标示变量或函数的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块中寻找其定义。它只要有两个作用：

- 当它与“C”一起连用的时候，如：extern "C" void fun(int a,int b);则告诉编译器在编译fun这个函数时候按着C的规矩去翻译，而不是C++的（这与C++的重载有关，C++语言支持函数重载，C语言不支持函数重载，函数被C++编译器编译后在库中的名字与C语言的不同）
- 当extern不与“C”在一起修饰变量或函数时，如：extern int g_Int；它的作用就是声明函数或全局变量的作用范围的关键字，其声明的函数和变量可以在本模块或其他模块中使用。记住它是一个声明不是定义!也就是说B模块(编译单元)要是引用模块(编译单元)A中定义的全局变量或函数时，它只要包含A模块的头文件即可,在编译阶段，模块B虽然找不到该函数或变量，但它不会报错，它会在连接时从模块A生成的目标代码中找到此函数。

2.static关键字的作用



- 修饰局部变量

    static修饰局部变量时，使得被修饰的变量成为静态变量，存储在静态区。存储在静态区的数据生命周期与程序相同，在main函数之前初始化，在程序退出时销毁。（无论是局部静态还是全局静态）



- 修饰全局变量

    全局变量本来就存储在静态区，因此static并不能改变其存储位置。但是，static限制了其链接属性。被static修饰的全局变量只能被该包含该定义的文件访问（即改变了作用域）。



- 修饰函数

     static修饰函数使得函数只能在包含该函数定义的文件中被调用。对于静态函数，声明和定义需要放在同一个文件夹中。

- 修饰成员变量
  用static修饰类的数据成员使其成为类的全局变量，会被类的所有对象共享，包括派生类的对象，所有的对象都只维持同一个实例。 因此，static成员必须在类外进行初始化(初始化格式：int base::var=10;)，而不能在构造函数内进行初始化，不过也可以用const修饰static数据成员在类内初始化。
- 修饰成员函数

     用static修饰成员函数，使这个类只存在这一份函数，所有对象共享该函数，不含this指针，因而只能访问类的static成员变量。静态成员是可以独立访问的，也就是说，无须创建任何对象实例就可以访问。例如可以封装某些算法，比如数学函数，如ln，sin，tan等等，这些函数本就没必要属于任何一个对象，所以从类上调用感觉更好，比如定义一个数学函数类Math，调用Math::sin(3.14);还可以实现某些特殊的设计模式：如Singleton；

最重要的特性：隐藏

    当同时编译多个文件时，所有未加static前缀的全局变量和函数都具有全局可见性，其它的源文件也能访问。利用这一特性可以在不同的文件中定义同名函数和同名变量，而不必担心命名冲突。static可以用作函数和变量的前缀，对于函数来讲，static的作用仅限于隐藏。

不可以同时用const和static修饰成员函数。

   C++编译器在实现const的成员函数的时候为了确保该函数不能修改类的实例的状态，会在函数中添加一个隐式的参数const this*。但当一个成员为static的时候，该函数是没有this指针的。也就是说此时const的用法和static是冲突的。我们也可以这样理解：两者的语意是矛盾的。static的作用是表示该函数只作用在类型的静态变量上，与类的实例没有关系；而const的作用是确保函数不能修改类的实例的状态，与类型的静态变量没有关系。因此不能同时用它们。

3.volatile的作用

     用来修饰变量的，表明某个变量的值可能会随时被外部改变，因此这些变量的存取不能被缓存到寄存器，每次使用需要重新读取。   

     假如有一个对象A里面有一个boolean变量a，值为true,现在有两个线程T1，T2访问变量a，T1把a改成了false后T2读取a，T2这时读到的值可能不是false，即T1修改a的这一操作，对T2是不可见的。发生的原因可能是，针对T2线程，为了提升性能，虚拟机把a变量置入了寄存器（即C语言中的寄存器变量），这样就会导致，无论T2读取多少次a，a的值始终为true，因为T2读取了寄存器而非内存中的值。声明了volatile或synchronized 后，就可以保证可见性，确保T2始终从内存中读取变量，T1始终在内存中修改变量。总结：防止脏读，增加内存屏障。

     也可参考该地址19问答案：http://www.sohu.com/a/166382914_538662

4.const的作用

- 定义变量为只读变量，不可修改
- 修饰函数的参数和返回值（后者应用比较少，一般为值传递）
- const成员函数（只需要在成员函数参数列表后加上关键字const，如char get() const;）可以访问const成员变量和非const成员变量，但不能修改任何变量。在声明一个成员函数时，若该成员函数并不对数据成员进行修改操作，应尽可能将该成员函数声明为const成员函数。
- 
  const对象只能访问const成员函数,而非const对象可以访问任意的成员函数,包括const成员函数.即对于class A，有const A a；那么a只能访问A的const成员函数。而对于：A b；b可以访问任何成员函数。
  使用const关键字修饰的变量，一定要对变量进行初始化

5.指针与引用的区别

- 指针只是一个变量，只不过这个变量存储的是一个地址；而引用跟原来的变量实质上是同一个东西，只不过是原变量的一个别名而已，不占用内存空间。
- 引用必须在定义的时候初始化，而且初始化后就不能再改变；而指针不必在定义的时候初始化，初始化后可以改变。
- 指针可以为空，但引用不能为空（这就意味着我们拿到一个引用的时候，是不需要判断引用是否为空的，而拿到一个指针的时候，我们则需要判断它是否为空。这点经常在判断函数参数是否有效的时候使用。）
- “sizeof 引用" = 指向变量的大小 ， "sizeof 指针"= 指针本身的大小
- 指针可以有多级，而引用只能是一级

6.new与malloc的区别

- malloc与free是C++/C语言的标准库函数，new/delete是C++的运算符。它们都可用于申请动态内存和释放内存。
- 对于非内部数据类型的对象而言，光用malloc/free无法满足动态对象的要求。对象在创建的同时要自动执行构造函数，对象在消亡之前要自动执行析构函数。
- new可以认为是malloc加构造函数的执行。new出来的指针是直接带类型信息的。而malloc返回的都是void指针。

7.C++的多态性

        多态性可以简单地概括为“一个接口，多种方法”，程序在运行时才决定调用的函数 。C++多态性主要是通过虚函数实现的，虚函数允许子类重写override(注意和overload的区别，overload是重载，是允许同名函数的表现，这些函数参数列表/类型不同）

    多态与非多态的实质区别就是函数地址是早绑定还是晚绑定。如果函数的调用，在编译器编译期间就可以确定函数的调用地址，并生产代码，是静态的，就是说地址是早绑定的。而如果函数调用的地址不能在编译器期间确定，需要在运行时才确定，这就属于晚绑定。

      在绝大多数情况下，程序的功能是在编译的时候就确定下来的，我们称之为静态特性。反之，如果程序的功能是在运行时刻才能确定下来的，则称之为动态特性。C++中，虚函数，抽象基类，动态绑定和多态构成了出色的动态特性。

    最常见的用法就是声明基类的指针，利用该指针指向任意一个子类对象，调用相应的虚函数，可以根据指向的子类的不同而实现不同的方法。

          a、编译时多态性：通过重载函数实现 
       b、运行时多态性：通过虚函数实现

    有关重载，重写，覆盖的区别请移步：https://www.cnblogs.com/LUO77/p/5771237.html

8.虚函数表

     请移步：https://www.cnblogs.com/LUO77/p/5771237.html

9.动态绑定与静态绑定

- 静态绑定发生在编译期，动态绑定发生在运行期；
- 对象的动态类型可以更改，但是静态类型无法更改；
- 要想实现动态，必须使用动态绑定；
- 在继承体系中只有虚函数使用的是动态绑定，其他的全部是静态绑定；
- 静态多态是指通过模板技术或者函数重载技术实现的多态，其在编译器确定行为。动态多态是指通过虚函数技术实现在运行期动态绑定的技术

动态绑定：有一个基类，两个派生类，基类有一个virtual函数，两个派生类都覆盖了这个虚函数。现在有一个基类的指针或者引用，当该基类指针或者引用指向不同的派生类对象时，调用该虚函数，那么最终调用的是该被指向对象对应的派生类自己实现的虚函数。

10.  虚函数表是针对类的还是针对对象的?同一个类的两个对象的虚函数表是怎么维护的?

编译器为每一个类维护一个虚函数表（本质是一个函数指针数组，数组里面存放了一系列函数地址 ），每个对象的首地址保存着该虚函数表的指针，同一个类的不同对象实际上指向同一张虚函数表。调用形式：*(this指针+调整量)虚函数在vftable内的偏移

在类内部添加一个虚拟函数表指针，该指针指向一个虚拟函数表，该虚拟函数表包含了所有的虚拟函数的入口地址，每个类的虚拟函数表都不一样，在运行阶段可以循此脉络找到自己的函数入口。纯虚函数相当于占位符， 先在虚函数表中占一个位置由派生类实现后再把真正的函数指针填进去。除此之外和普通的虚函数没什么区别。

在单继承形式下，子类的完全获得父类的虚函数表和数据。子类如果重写了父类的虚函数（如fun），就会把虚函数表原本fun对应的记录（内容MyClass::fun）覆盖为新的函数地址（内容MyClassA::fun），否则继续保持原本的函数地址记录。

使用这种方式，就可以实现多态的特性。假设我们使用如下语句：

    MyClass*pc=new MyClassA;
    pc->fun();  1

因为虚函数表内的函数地址已经被子类重写的fun函数地址覆盖了，因此该处调用的函数正是MyClassA::fun，而不是基类的MyClass::fun。

如果使用MyClassA对象直接访问fun，则不会出发多态机制，因为这个函数调用在编译时期是可以确定的，编译器只需要直接调用MyClassA::fun即可。

注：对象不包含虚函数表，只有虚指针，类才包含虚函数表，派生类会生成一个兼容基类的虚函数表

详情可以参考：http://www.cnblogs.com/fanzhidongyzby/archive/2013/01/14/2859064.html

11.智能指针怎么实现？什么时候改变引用计数？

- 构造函数中计数初始化为1；
- 拷贝构造函数中计数值加1；
- 赋值运算符中，左边的对象引用计数减一，右边的对象引用计数加一；
- 析构函数中引用计数减一；
- 在赋值运算符和析构函数中，如果减一后为0，则调用delete释放对象。

12.内联函数，宏定义和普通函数的区别

- 内联函数要做参数类型检查，这是内联函数跟宏相比的优势
- 宏定义是在预编译的时候把所有的宏名用宏体来替换，简单的说就是字符串替换， 内联函数则是在编译的时候进行代码插入，编译器会在每处调用内联函数的地方直接把内联函数的内容展开，这样可以省去函数的调用的压栈出栈的开销，提高效率。
- 内联函数是指嵌入代码，就是在调用函数的地方不是跳转，而是把代码直接写到那里去。对于短小简单的代码来说，内联函数可以带来一定的效率提升，而且和C时代的宏函数相比，内联函数 更安全可靠。可是这个是以增加空间消耗为代价的
- const与#define的区别：宏在预处理阶段替换，const在编译阶段替换；宏没有类型，不做安全检查，const有类型，在编译阶段进行安全检查

13. C++内存管理

栈**:** 存放函数参数以及局部变量,在出作用域时,将自动被释放.栈内存分配运算内置于处理器的指令集中,效率很高,但分配的内存容量有限.



堆**:**new分配的内存块(包括数组,类实例等),需delete手动释放.如果未释放,在整个程序结束后,OS会帮你回收掉.



自由存储区:malloc分配的内存块,需free手动释放.它和堆有些相似.



全局/静态区:保存自动全局变量和static变量（包括static全局和局部变量）。静态区的内容在整个程序的生命周期内都存在，有编译器在编译的时候分配（数据段（存储全局数据和静态数据）和代码段（可执行的代码/只读常量））。



常量存储区:常量(const)存于此处,此存储区不可修改.







栈与堆的区别：       

管理方式不同: 栈是编译器自动管理的,堆需手动释放

空间大小不同: 在32位OS下,堆内存可达到4GB的的空间,而栈就小得可怜.(VC6中,栈默认大小是1M,当然,你可以修改它)

能否产生碎片不同:对于栈来说,进栈/出栈都有着严格的顺序(先进后出),不会产生碎片;而堆频繁的new/delete,会造成内存空间的不连续,容易产生碎片.

生长方向不同:栈向下生长,以降序分配内存地址;堆向上生长,以升序分配内在地址.

分配方式不同:堆动态分配,无静态分配;栈分为静态分配和动态分配,比如局部变量的分配,就是动态分配(alloca函数),函数参数的分配就是动态分配(我想的…).

分配效率不同:栈是系统提供的数据结构,计算机会在底层对栈提供支持,进栈/出栈都有专门的指令,这就决定了栈的效率比较高.堆则不然,它由C/C++函数库提供,机制复杂,堆的效率要比栈低得多.

可以看出,栈的效率要比堆高很多,所以,推荐大家尽量用栈.不过,虽然栈有如此多的好处,但远没有堆使用灵活.



14 . 常用的设计模式

     单例模式：保证一个类仅有一个实例，并提供一个访问它的全局访问点；

     工厂模式：定义一个用于创建对象的接口，让子类决定实例化哪一个类。

     参考：https://www.cnblogs.com/Y1Focus/p/6707121.html

               https://www.zhihu.com/question/34574154?sort=created



15.手写strcpy，memcpy，strcat，strcmp等函数

https://blog.csdn.net/gao1440156051/article/details/51496782

https://blog.csdn.net/wilsonboliu/article/details/7919773



16.i++是否为原子操作？

不是。操作系统原子操作是不可分割的，在执行完毕不会被任何其它任务或事件中断，分为两种情况（两种都应该满足）

 （1） 在单线程中， 能够在单条指令中完成的操作都可以认为是" 原子操作"，因为中断只能发生于指令之间。

 （2） 在多线程中，不能被其它进程（线程）打断的操作就叫原子操作。

i++分为三个阶段：

内存到寄存器
寄存器自增
写回内存
这三个阶段中间都可以被中断分离开.



17.有关数组，指针，函数的三者结合问题

数组指针和指针数组的区别：https://blog.csdn.net/men_wen/article/details/52694069

右左法则的说明：http://www.cnblogs.com/zhangjing0502/archive/2012/06/08/2542059.html

指针常量和常量指针：https://www.zhihu.com/question/19829354

                                 https://blog.csdn.net/xingjiarong/article/details/47282563

注意：所谓指向常量的指针或引用（即常量引用、常量指针），不过是指针或引用“自以为是”罢了，它们觉得自己指向了常量，所以自觉地不去改变所指对象的值，但这些对象却可以通过其他途径改变。

const int *a;  等价于int const *a;    const在前面所以内容不可以改变，但是指针指向可以改变。也就是常量指针

int *const a;  表示的是指针指向不可改变，但是指针所存放的内容可以改变，也即是指针常量

补充：



18.C++中类与结构体的区别？

- 最本质的一个区别就是默认的访问控制： struct作为数据结构的实现体，它默认的数据访问控制是public的，而class作为对象的实现体，它默认的成员变量访问控制是private的。
- “class”这个关键字还用于定义模板参数，就像“typename”。但关键字“struct”不用于定义模板参数。
- 

19.析构函数的作用？

析构函数是用来释放所定义的对象中使用的指针，默认的析构函数不用显示调用，自建的析构函数要在程序末尾调用。

如果你的类里面只用到的基本类型，如int char double等，系统的默认析构函数其实什么都没有做

但如果你使用了其他的类如vector，string等，系统的默认析构函数就会调用这些类对象的析构函数

如果是自己写析构函数的话，如果你的类里面分配了系统资源，如new了内存空间，打开了文件等，那么在你的析构函数中就必须释放相应的内存空间和关闭相关的文件；这样系统就会自动调用你的析构函数释放资源，避免内存泄漏



例如：

    class A
    {
    private:
        char *data;
    public:
        A()
        {
            data = new char[10];
        }
        ~A()
        {
            delete[] data;
        }
    };1

A  a;
a 中将 new 10个 char
当 a 这个变量消亡的时候，将自动执行 ~A()，释放空间

对象消亡时，自动被调用，用来释放对象占用的空间，避免内存泄漏

20.虚函数的作用？

          虚函数可以让成员函数操作一般化，用基类的指针指向不同的派生类的对象时，基类指针调用其虚成员函数，则会调用其真正指向对象的成员函数，而不是基类中定义的成员函数（只要派生类改写了该成员函数）。若不是虚函数，则不管基类指针指向的哪个派生类对象，调用时都会调用基类中定义的那个函数。虚函数是C++多态的一种表现，可以进行灵活的动态绑定。

     重点可参考：https://www.cnblogs.com/wangxiaobao/p/5850949.html

                     http://www.cnblogs.com/fanzhidongyzby/archive/2013/01/14/2859064.html

21.操作系统和编译器如何区分全局变量和局部变量？

       操作系统只管调度进程，编译器通过内存分配的位置来知道的，全局变量分配在全局数据段并且在程序开始运行的时候被加载。局部变量则分配在栈里面 。

22. Makefile文件的作用？

   makefile关系到了整个工程的编译规则。一个工程中的源文件不计数，其按类型、功能、模块分别放在若干个目录中，makefile定义了一系列的规则来指定，哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至于进行更复杂的功能操作，因为makefile就像一个Shell脚本一样，其中也可以执行操作系统的命令。

23.结构体和联合体的区别？

      结构和联合都是由多个不同的数据类型成员组成, 但在任何同一时刻, 联合中只存放了一个被选中的成员（所有成员共用一块地址空间）, 而结构的所有成员都存在（不同成员的存放地址不同）。 

     对于联合的不同成员赋值, 将会对其它成员重写, 原来成员的值就不存在了, 而对于结构的不同成员赋值是互不影响的。

24.列表初始化问题？

      使用初始化列表主要是基于性能问题，对于内置类型，如int, float等，使用初始化列表和在构造函数体内初始化差别不是很大；但是对于类类型来说，最好使用初始化列表。这样就可以直接调用拷贝构造函数初始化，省去了一次调用默认构造函数的过程。

    struct Test1
    {
        Test1() // 无参构造函数
        { 
            cout << "Construct Test1" << endl ;
        }
        Test1(const Test1& t1)  // 拷贝构造函数
        {
            cout << "Copy constructor for Test1" << endl ;
            this->a = t1.a ;
        }
        Test1& operator = (const Test1& t1) // 赋值运算符
        {
            cout << "assignment for Test1" << endl ;
            this->a = t1.a ;
            return *this;
        }
        int a ;
    };
    
    struct Test2           //普通初始化
    {
        Test1 test1 ;
        Test2(Test1 &t1)
        {
            test1 = t1 ;
        }
    };1
    struct Test2          //2.列表初始化
    {
        Test1 test1 ;
        Test2(Test1 &t1):test1(t1){}
    }1
    Test1 t1 ;            //调用
    Test2 t2(t1) ; 1

普通初始化：



列表初始化：



下列情况一定要使用初始化成员列表

- 常量成员，因为常量只能初始化不能赋值，所以必须放在初始化列表里面
- 引用类型，引用必须在定义的时候初始化，并且不能重新赋值，所以也要写在初始化列表里面
- 需要初始化的数据成员是对象的情况
  参考地址：https://www.cnblogs.com/weizhixiang/p/6374430.html 

25. 重载与重写的区别？

    从定义上来说：重载：是指允许存在多个同名函数，而这些函数的参数表不同（或许参数个数不同，或许参数类型不同，或许两者都不同）。重写：是指子类重新定义父类虚函数的方法。

   从实现原理上来说：重载：编译器根据函数不同的参数表，对同名函数的名称做修饰，然后这些同名函数就成了不同的函数。重写：当子类重新定义了父类的虚函数后，父类指针根据赋给它的不同的子类指针，动态的调用属于子类的该函数，这样的函数调用在编译期间是无法确定的（调用的子类的虚函数的地址无法给出）。

   补充：“隐藏”是指派生类的函数屏蔽了与其同名的基类函数。规则如下： 
（1）如果派生类的函数与基类的函数同名，但是参数不同。此时，不论有无virtual关键字，基类的函数将被隐藏（注意别与重载混淆）。 
（2）如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有virtual 关键字。此时，基类的函数被隐藏（注意别与覆盖混淆）。

26.类型安全以及C++中的类型转换？

     类型安全很大程度上可以等价于内存安全，类型安全的代码不会试图访问自己没被授权的内存区域。C只在局部上下文中表现出类型安全，比如试图从一种结构体的指针转换成另一种结构体的指针时，编译器将会报告错误，除非使用显式类型转换。然而，C中相当多的操作是不安全的。

 详情可以移步：https://blog.csdn.net/chengonghao/article/details/50974022

   四种类型转换：

- static_cast <T*> (content)  静态转换.在编译期间处理，可以实现C++中内置基本数据类型之间的相互转换。如果涉及到类的话，static_cast只能在有相互联系的类型中进行相互转换,不一定包含虚函数。
- dynamic_cast<T*>(content) 动态类型转换;也是向下安全转型;是在运行的时候执行;基类中一定要有虚函数，否则编译不通过。在类层次间进行上行转换时（如派生类指针转为基类指针），dynamic_cast和static_cast的效果是一样的。在进行下行转换时（如基类指针转为派生类指针），dynamic_cast具有类型检查的功能，比static_cast更安全。
- const_cast<T*>(content) 去常转换;编译时执行;
- reinterpret_cast<T*>(content) 重解释类型转换;

详情可以移步：https://blog.csdn.net/u010025211/article/details/48626687

                        https://blog.csdn.net/xtzmm1215/article/details/46475565

                        https://blog.csdn.net/xingkongfenqi/article/details/49148885

27.内存对齐的原则以及作用？

- 结构体内的成员按自身长度自对齐（32位机器上，如char=1，short=2，int=4，double=8），所谓自对齐是指该成员的起始地址必须是它自身长度的整数倍。如int只能以0,4,8这类地址开始。
- 结构体的总大小为结构体的有效对齐值的整数倍（默认以结构体中最长的成员长度为有效值的整数倍，当用#pragrma pack（n）指定时，以n和结构体中最长的成员的长度中较小者为其值）。即sizeof的值，必须是其内部最大成员的整数倍，不足的要补齐。

 例如：

    class A
    {
        char c;
        int a;
        char d;
    };
    
    cout << sizeof(A) << endl;
    class B
    {
        char c;
        char d;
        int a;
    };
    cout << sizeof(B) << endl;1

sizeof（A）=12，sizeof（B）=8；

因为左边是1+（3）+4+1+（3）=12，而右边是1+1+（2）+4=8。括号中为补齐的字节。

内存对齐的作用：

1、平台原因(移植原因)：不是所有的硬件平台都能访问任意地址上的任意数据的；某些硬件平台只能在某些地址处取某些特定类型的数据，否则抛出硬件异常。

2、性能原因：经过内存对齐后，CPU的内存访问速度大大提升。

详情可以移步：https://blog.csdn.net/chy19911123/article/details/48894579

28.关键字registr，typdef的作用？

register关键字的作用：

请求CPU尽可能让变量的值保存在CPU内部的寄存器中，减去CPU从内存中抓取数据的时间，提高程序运行效率。

使用register关键字应注意什么？

1.只有局部变量才可以被声明用register修饰

（register不能修饰全局变量和函数的原因：全局变量可能被多个进程访问，而用register修饰的变量，只能被当前进程访问）

2.不能用取地址获取用register修饰的变量的地址（原因：变量保存在寄存器中，而取地址获取的地址的是内存的地址）

\3. 用register修饰的变量一定要是CPU所接受的数据类型



typedef关键字的作用：

给数据类型定义一个新名字，

\1.  提高了移植性

\2.  简化复杂的类型声明，提高编码效率

\3.  解释数据类型的作用

29.什么情况下需要将析构函数定义为虚函数？

    当基类指针指向派生类的对象（多态性）时。如果定义为虚函数，则就会先调用该指针指向的派生类析构函数，然后派生类的析构函数再又自动调用基类的析构函数，这样整个派生类的对象完全被释放。如果析构函数不被声明成虚函数，则编译器实施静态绑定，在删除基类指针时，只会调用基类的析构函数而不调用派生类析构函数，这样就会造成派生类对象析构不完全。所以，将析构函数声明为虚函数是十分必要的。

  详情可以移步：https://blog.csdn.net/jiadebin890724/article/details/7951461

30.有关纯虚函数的理解？

     纯虚函数是为你的程序制定一种标准，纯虚函数只是一个接口，是个函数的声明而已，它要留到子类里去实现。

    class A{
    protected:
    
    void foo();//普通类函数
    virtual void foo1();//虚函数
    virtual void foo2() = 0;//纯虚函数
    
    
    }1

带纯虚函数的类抽象类，这种类不能直接生成对象，而只有被继承，并重写其虚函数后，才能使用。
    虚函数是为了继承接口和默认行为

    纯虚函数只是继承接口，行为必须重新定义

   （在很多情况下，基类本身生成对象是不合情理的。例如，动物作为一个基类可以派生出老虎、孔雀等子类，但动物本身生成对象明显不合常  理。所以引入了纯虚函数的概念）

    详情可以参考：https://blog.csdn.net/ybhjx/article/details/51788396

30.基类指针指向派生类，派生类指针指向基类？

    基类指针可以指向派生类对象，从而实现多态，例如：

    #include <iostream>  
    
    using namespace std;  
    class Shape {  
    
    public:  
        virtual double area() const = 0; //纯虚函数  
    };  
    
    class Square : public Shape {  
        double size;  
    public:  
        Square(double s) {  
            size = s;  
        }  
        virtual double area() const {  
           return size * size;  
        }  
    };  
    
    class Circle : public Shape {  
        double radius;  
    public:  
        Circle(double r) {  
            radius = r;  
        } 
        virtual double area() const {  
           return 3.14159 * radius * radius;  
        }  
    };  
    
     int main()  
     {  
         Shape* array[2]; //定义基类指针数组  
         Square Sq(2.0);  
         Circle Cir(1.0);  
        array[0] = &Sq;  
        array[1] =&Cir;  
    
        for (int i = 0; i < 2; i++) /  
        {  
            cout << array[i]->area() << endl;  
    
        }  
    
        return 0;  
    
    }  1

     上面的不同对象Sq,Cir(来自继承同一基类的不同派生类)接受同一消息（求面积，来自基类的成员函数area()）,但是却根据自身情况调用不同的面积公式（执行了不同的行为，它是通过虚函数实现的）。我们可以理解为，继承同一基类的不同派生对象，对来自基类的同一消息执行了不同的行为，这就是多态，它是通过继承和虚函数实现的。而接受同一消息的实现就是基于基类指针。 

     但是要注意的是，这个指针只能用来调用基类的成员函数。

     如果试图通过基类指针调用派生类才有的成员函数，则编译器会报错。

     为了避免这种错误，必须将基类指针强制转化为派生类指针。然后派生类指针可以用来调用派生类的功能。这称为向下强制类型转换，这是一种潜在的危险操作。

     派生类指针不可以指向基类对象,例如：

         有个people类是基类，成员有姓名和身份证号，有个派生类学生student，添加了成员学号，现在如果你说的这个情况成立student的指针----pt让他指向people成员t，则t只有两个成员变量，而*pt有3个，现在pt->学号这个变量在pt下是可以使用的，但它指向的实体却没有这个变量，所以出错，于是C++直接就避免了这样的隐式转换。
         所以根据上述信息我们可以知道：
        进行上行转换（把派生类的指针或引用转换成基类表示）是安全的；
    　 进行下行转换（把基类指针或引用转换成派生类表示）是不安全的。

    参考链接：https://blog.csdn.net/flyingbird_sxf/article/details/41358737

                     https://www.cnblogs.com/rednodel/p/5800142.html

   31. 继承机制中引用和指针之间如何转换？ 
    基类——>派生类：用dynamic_cast转换（显示转换），首先检查基类指针（引用）是否真正指向一个派生类对象，然后再做相应处理，对指针进行dynamic_cast，成功返回派生类对象，失败返回空指针，对引用进行dynamic_cast，成功返回派生类对象，失败抛出一个异常。 不允许隐式转换。
    派生类——>基类：可以用dynamic_cast或者直接进行类型转换（直接赋值）。

    32.c语言和c++有什么区别？ 
    C语言是结构化的编程语言，它是面向过程的，而C++是面向对象的。 
    封装：将数据和函数等集合在一个单元中（即类）。被封装的类通常称为抽象数据类型。封装的意义在于保护或者防止代码（数据）被我们无意中破坏。 
    继承：继承主要实现重用代码，节省开发时间。它可以使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展。 
    多态：同一操作作用于不同的对象，可以有不同的解释，产生不同的执行结果。在运行时，可以通过指向派生类的基类指针，来调用实现派生类中的方法。有编译时多态和运行时多态。

   33.C++中的公有，私有，保护的问题？ 

      	类   	类对象 	公有继承派生类	私有继承派生类	保护继承派生类	公有继承派生类对象	私有继承派生类对象	保护继承派生类对象
  公有成员	√   	√   	√      	√      	√      	√        	X        	X        
  私有成员	√   	X   	X      	X      	X      	X        	X        	X        
  保护成员	√   	X   	√      	√      	√      	X        	X        	X        

   √：代表可以访问，X代表不能访问。     

 参考链接：https://zhidao.baidu.com/question/551075894.html

34.如何实现类对象只能静态分配或动态分配？

 C++中建立类的对象有两种方式：

（1）静态建立，例如 A a;

     静态建立一个类对象，就是由编译器为对象在栈空间中分配内存。使用这种方法，是直接调用类的构造函数。

（2）动态建立，例如 A* p = new A();

     动态建立一个类对象，就是使用new运算符为对象在堆空间中分配内存。这个过程分为两步：第一步执行operator new( )函数，在堆空间中搜索一块内存并进行分配；第二步调用类的构造函数构造对象。这种方法是间接调用类的构造函数。

只能动态分配：  

      其实，编译器在为类对象分配栈空间时，会先检查类的析构函数的访问性（其实不光是析构函数，只要是非静态的函数，编译器都会进行检查）。如果类的析构函数在类外部无法访问，则编译器拒绝在栈空间上为类对象分配内存。因此，可以将析构函数设为private，这样就无法在栈上建立类对象了。但是为了子类可以继承，最好设置成protected。



1. class A  
2. {  
3. protected:  
4.      A(){}  
5.      ~A(){}  
6. public:  
7.      static A* create(){return new A();}  
8.      void destory(){delete this;}  
9. };  



只能静态分配：

    只有使用new运算符，对象才会被建立在堆上。因此只要限制new运算符就可以实现类对象只能建立在栈上。可以将new运算符设为私有。



1. class A  
2. {  
3. private:  
4.      void* operator new(size_t t){}            //注意函数的第一个参数和返回值都是固定的  
5.      void  operator delete(void* ptr)()        //重载了new就需要重载delete  
6. public:  
7.      A(){}  
8.      ~A(){}  
9. };  

35.explicit关键字的作用？



C++中， 一个参数的构造函数(或者除了第一个参数外其余参数都有默认值的多参构造函数)， 承担了两个角色。 1 是个构造器 ，2 是个默认且隐含的类型转换操作符。

所以， 有时候在我们写下如 AAA = XXX， 这样的代码， 且恰好XXX的类型正好是AAA单参数构造器的参数类型， 这时候编译器就自动调用这个构造器， 创建一个AAA的对象。

这样看起来好象很酷， 很方便。 但在某些情况下（见下面权威的例子）， 却违背了我们（程序员）的本意。 这时候就要在这个构造器前面加上explicit修饰， 指定这个构造器只能被明确的调用/使用， 不能作为类型转换操作符被隐含的使用。

    class Test1
    {
    public:
        Test1(int n)
        {
            num=n;
        }//普通构造函数
    private:
        int num;
    };
    
    class Test2
    {
    public:
        explicit Test2(int n)
        {
            num=n;
        }//explicit(显式)构造函数
    
    private:
       int num;
    };
    
    int main()
    {
        Test1 t1=12;//隐式调用其构造函数,成功
        Test2 t2=12;//编译错误,不能隐式调用其构造函数
        Test2 t2(12);//显式调用成功
    
        return 0;
    
    
    
    }1
    

Test1的构造函数带一个int型的参数，代码23行会隐式转换成调用Test1的这个构造函数。而Test2的构造函数被声明为explicit（显式），这表示不能通过隐式转换来调用这个构造函数，因此代码24行会出现编译错误。

普通构造函数能够被隐式调用。而explicit构造函数只能被显式调用。

36.内存溢出，内存泄漏的原因？

    内存溢出是指程序在申请内存时，没有足够的内存空间供其使用。原因可能如下：

- 内存中加载的数据量过于庞大，如一次从数据库取出过多数据
- 代码中存在死循环或循环产生过多重复的对象实体
- 递归调用太深，导致堆栈溢出等
- 内存泄漏最终导致内存溢出

    内存泄漏是指向系统申请分配内存进行使用（new），但是用完后不归还（delete），导致占用有效内存。常见的几种情况：

   （1） 在类的构造函数和析构函数中没有匹配的调用new和delete函数

        两种情况下会出现这种内存泄露：一是在堆里创建了对象占用了内存，但是没有显示地释放对象占用的内存；二是在类的构造函数中动态的分配了内存，但是在析构 函数中没有释放内存或者没有正确的释放内存

   （2） 在释放对象数组时在delete中没有使用方括号

      方括号是告诉编译器这个指针指向的是一个对象数组，同时也告诉编译器正确的对象地址值病调用对象的析构函数，如果没有方括号，那么这个指针就被默认为只指向一个对象，对象数组中的其他对象的析构函数就不会被调用，结果造成了内存泄露。

    （3）没有将基类的析构函数定义为虚函数

       当基类指针指向子类对象时，如果基类的析构函数不是virtual，那么子类的析构函数将不会被调用，子类的资源没有正确是释放，因此造成内存泄露

     参考链接： https://blog.csdn.net/hyqwmxsh/article/details/52813307    

缓冲区溢出（栈溢出）

     程序为了临时存取数据的需要，一般会分配一些内存空间称为缓冲区。如果向缓冲区中写入缓冲区无法容纳的数据，机会造成缓冲区以外的存储单元被改写，称为缓冲区溢出。而栈溢出是缓冲区溢出的一种，原理也是相同的。分为上溢出和下溢出。其中，上溢出是指栈满而又向其增加新的数据，导致数据溢出；下溢出是指空栈而又进行删除操作等，导致空间溢出。



37.auto_ptr类与shared_ptr类？

      从c++11开始, auto_ptr已经被标记为弃用, 常见的替代品为shared_ptr。shared_ptr的不同之处在于引用计数, 在复制(或赋值)时不会像auto_ptr那样直接转移所有权。 两者都是模板类，却可以像指针一样去使用。只是在指针上面的一层封装。

      auto_ptr实际也是一种类, 拥有自己的析构函数, 生命周期结束时能自动释放资源，正因为能自动释放资源, 特别适合在单个函数内代替new/delete的调用, 不用自己调用delete，也不用担心意外退出造成内存的泄漏。

   atuo_ptr的缺陷：

- auto_ptr不能共享所有权，即不要让两个auto_ptr指向同一个对象（因为它采用的是转移语义的拷贝，原指针会变为NULL）。
- auto_ptr不能管理对象数组（因为它内部的析构函数调用的是delete而不是delete[]）。
- auto_ptr不能作为容器对象，STL容器中的元素经常要支持拷贝，赋值等操作，在这过程中auto_ptr会传递所有权。
  详情原因可以参考：https://blog.csdn.net/uestclr/article/details/51316001

                                    https://blog.csdn.net/kezunhai/article/details/38514823

     

     shared_ptr 使用引用计数的方式来实现对指针资源的管理。同一个指针资源，可以被多个 shared_ptr 对象所拥有，直到最后一个 shared_ptr 对象析构时才释放所管理的对象资源。

      可以说，shared_ptr 是最智能的智能指针，因为其特点最接近原始的指针。不仅能够自由的赋值和拷贝，而且可以安全的用在标准容器中。

38.   有4种情况，编译器必须为未声明的constructor的classes合成一个default constructor：

l  “带有默认构造函数”的成员对象

l  “带有默认构造函数”的基类

l  “带有虚函数”的类

l  “带有虚拟基类”的类

被合成的构造函数只能满足编译器（而非程序员）的需要。在合成默认的构造函数中，只有基类的子对象和成员对象会被初始化，其他非静态的数据成员（如整数，指针等）都不会被初始化。

所以并不是任何的类如果没有定义默认的构造函数，都会被合成一个出来。

39.

虚基类

在C++中,如果在多条继承路径上有一个公共的基类,那么在这些路径中的某几条路径的汇合处,这个公共的基类就会产生多个实例（从而造成二义性）.如果想使这个公共的基类只产生一个实例,则可将这个基类说明为虚基类. 这要求在从base类派生新类时,使用关键字virtual将base类说明为虚基类.

用例子说明吧。

class base{protected:int b};
clase base1:public base{..};
clase base2:public base{..};
clase derived:public base1,public base2 {..};
derived d;
d.b //错误.

d.base::b //错误. 因为不知是用d.base1::b还是d.base2::b

class base{protected:int b..};
clase base1:virtual public base{..}; //说明base为base1虚基类
clase base2:virtual public base{..}; //说明base为base2虚基类
clase derived:public base1,public base2 {..};
derived d;
d.b //对.
d.base::b //对. 因为d.base::b和d.base1::b还是d.base2::b都是引用同一虚基类成员b,具有相同的值.



40. 模板的特例化

引入原因：编写单一的模板，它能适应大众化，使每种类型都具有相同的功能，但对于某种特定类型，如果要实现其特有的功能，单一模板就无法做到，这时就需要模板特例化。 
定义：是对单一模板提供的一个特殊实例，它将一个或多个模板参数绑定到特定的类型或值上。

函数模板特例化：必须为原函数模板的每个模板参数都提供实参，且使用关键字template后跟一个空尖括号对<>,表明将原模板的所有模板参数提供实参。

     template <typename T>  
    
     void fun(T a)  
    
    {  
    
        cout << "The main template fun(): " << a << endl;  
    
    }  
    
    template <>   // 对int型特例化  
    
    void fun(int a)  
    
    {  
    
    cout << "Specialized template for int type: " << a << endl;  
    
    }  
    
      
    
    int main()  
    {  
        fun<char>('a');  
        fun<int>(10);  
        fun<float>(9.15);  
        return 0;  
    }  
    
    



对于除int型外的其他数据类型，都会调用通用版本的函数模板fun(T a)；对于int型，则会调用特例化版本的fun(int a)。注意，一个特例化版本的本质是一个实例，而非函数的重载。因此，特例化不影响函数匹配。

 

类模板的特例化：

\1.   template <typename T>  

\2.   class Test{  

\3.   public:  

\4.       void print(){  

\5.           cout << "General template object" << endl;  

\6.       }  

\7.   };  

\8.     

\9.   template<>   // 对int型特例化  

\10. class Test<int>{  

\11. public:  

\12.     void print(){  

\13.         cout << "Specialized template object" << endl;  

\14.     }  

\15. };  

另外，与函数模板不同，类模板的特例化不必为所有模板参数提供实参。我们可以只指定一部分而非所有模板参数，这种叫做类模板的偏特化 或部分特例化（partial specialization）。例如，C++标准库中的类vector的定义：

[cpp] view plain copy

\1.   template <typename T, typename Allocator>  

\2.   class vector  

\3.   {  

\4.       /....../  

\5.   };  

\6.     

\7.   // 部分特例化  

\8.   template <typename Allocator>  

\9.   class vector<bool, Allocator>  

{  

 /....../  

};  

在vector这个例子中，一个参数被绑定到bool类型，而另一个参数仍未绑定需要由用户指定。注意，一个类模板的部分特例化版本仍然是一个模板，因为使用它时用户还必须为那些在特例化版本中未指定的模板参数提供实参。 

参考链接：https://blog.csdn.net/lisonglisonglisong/article/details/38057367