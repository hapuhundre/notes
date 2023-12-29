## 1. 基本概念

模板类型：函数模板, 类模板，可变参模板以及别名模板；

模板参数：类型模板参数，非类型模板参数，双重模板参数；

特化(specialization): 偏特化，（显式）全特化

实例化（instantiation）：显示/隐式

### 1.1 模板参数

```c++
// 非类型模板参数 non-type template parameters
template <typename T, size_t S>
class buffer
{
    T data_[S];

public:
    constexpr T const *data() const { return data_; }
    constexpr T &operator[](size_t const index)
    {
        return data_[index];
    }
    constexpr T const &operator[](size_t const index) const
    {
        return data_[index];
    }
};

buffer<int, 64> bf; // 一个编译期就可以优化了的对象，和buffer<int,32>编译器会生成两套不同的源码
```

当然，非模板参数也可以为指针，来传递函数等行为：

```c++
struct device
{
    virtual void output() = 0;
    virtual ~device() {}
};

template <void (*action)()>
struct smart_device : device
{
    void output() override
    {
        (*action)();
    }
};

void action1(){cout << "haha\n";}
void action2(){cout << "yoxi\n";}

smart_device<action1> sd1;
smart_device<action2> sd2;
sd1.output();
sd2.output();

// 下面这种写法在c++20才比较完备的支持，c++17可能会报错，例如foo<42.0> f; 这种float类型，string类型等等
template <auto x>
struct foo{ /* ... */ };
```

双重模板参数(Template template parameters)实例如下:

```c++
template <typename T>
class simple_wrapper
{
public:
    simple_wrapper(const T& v): value(v){}
    T value;
};

template <typename T>
class fancy_wrapper
{
public:
    fancy_wrapper(T const v) : value(v){}
    
    T const &get() const { return value; }
    
    template <typename U>
    U as() const
    {
        return static_cast<U>(value);
    }

private:
    T value;
};

template <typename T, typename U,
          template <typename> typename W = fancy_wrapper> // 双重模板，将T传递给了 W<T>, 同时也是一个默认模板参数
class wrapping_pair
{
public:
    wrapping_pair(T const a, U const b) : item1(a), item2(b)
    {
    }
    W<T> item1;
    W<U> item2;
};

wrapping_pair<int, double> p1(42, 42.0); // fancy_wrapper
cout << p1.item1.get() << ", " << p1.item2.get() << "\n";

wrapping_pair<int, double, simple_wrapper> p2(36, 36.0); // simple_wrapper
cout << p2.item1.value << ", " << p2.item2.value << '\n';
```

注意，默认模板参数应该放在后面，声明和定义是不必重复设置，跟默认形参是一个规则。

### 1.2 模板实例化

所谓的隐式实例化就是在需要的时候（例如调用，初始化），编译器才去实例化类型对象。

显示的实例化声明，例如下述代码：

```c++
template <typename T>
class foo
{
    struct bar {};
    T f(bar const arg) {
        return {};
    }
};

template int foo<int>::f(foo<int>::bar);
```

上述做法可以减少分发了的库的编译时间，和编译后文件的大小。



### 1.3 模板特化

特化就是特指的意思，例如下述模板：

```c++
template  <typename T>
struct is_floating_point{
    constexpr static bool value = false;
};
```

可以通过以下语义来定义特化后的模板：

```c++
template <>
struct is_floating_point<float>{
    constexpr static bool value = true;
}
```

