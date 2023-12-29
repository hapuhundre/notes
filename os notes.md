读寄存器，改寄存器的变量，写入变量



编译器带来非顺序的执行方式在多线程下带来新的问题：

```c++
bool done{false};
while(1) {
    if(!done) {
        break;
    }
}
```

volatile可以组织编译器优化。

多线程放弃了执行顺序。

