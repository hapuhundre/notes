# functional programming of cpp

Last Edited: November 19, 2021 3:25 PM
Tags: cpp

## Chapter 3

此代码段中，有一个简单的函数模板，该模板带有一个对象，以及应该在该对象上调用的函数。 可以完美地将object参数转发给函数； 并使用decltype（auto）作为返回类型，可以完美地将函数的结果转发回调用方。这样写的好处是啥呢？感觉就是给function起个别名?

```cpp
template <typename Object, typename Function>
decltype(auto) call_on_object(Object&& object, Function function)
{
    return function(std::forward<Object>(object));
}
```

```cpp
class older_than { /* 这个older_than 代码taste不错，我喜欢 */
public:
  older_than(int limit) : m_limit(limit)
  {
  }
  template <typename T>
  bool operator()(T&& object) const
  {
    return std::forward<T>(object).age()> m_limit; /* 模板一定得带有age属性 */
  }
private:
  int m_limit;
};
// 计算 
std::count_if(persons.cbegin(), persons.cend(),older_than<person_t>(42));
```

### **3.2 Lambdas and closures**

很重要的一节， lambda结构形式如下：

![](E:/func_cpp_1.png)

Head: 要在Body里面使用的参数列表;

Arg: 函数形参

今天写了个lambda，需求是在一个函数里内嵌一个会重复两次的函数（异常日志）代码如下：

```cpp
AgentGlobalReplan(std::string serial)
{
  // plan current ts
  auto agent = agents_manager_->GetAgentPtr(serial);
  auto start = dm_.GetNode(agent->feedback.timestamp);
  auto goal  = dm_.GetNode(agent->current_task_simplex.destination.timestamp);
  //--------------------
  using ID = decltype(start.id);
  std::function<void(ID,ID)> NodeCheck = [](ID st, ID gl)
  { /* 要复用lambda，必须使用std::fuction来进行赋值 */
    if(st < 0 || gl < 0)
    {
      std::string msg = "FSB-AGP global replan error";
      msg += (" goal(" + std::to_string(gl) + ")");
      LOG(INFO) << msg;
      throw std::runtime_error(msg);
    }
  };
  //----------------
  NodeCheck(start.id, goal.id);
  // ...
}
```

C++函数内部是不允许内嵌定义函数的，lambda可以实现这一点。

创建一个lambda不是写了一个函数，而是本质上创建了一个类（毕竟可以传this指针）。除了创建类之外，对lambda表达式求值还会创建该类的实例，称为**闭包**：该对象包含某种状态或环境以及应在该状态上执行的代码。

一般情况下，lambda 的Head参数列表中的参数为const，但可以通过mutable来改变Head中的参数值，eg:

```cpp
int count = 0;
std::vector<std::string> words{"An", "ancient", "pond"};
std::for_each(words.cbegin(), words.cend(),
  [count](const std::string& word) mutable                        
  {
    if (isupper(word[0])) {
      std::cout << word<< " " << count <<std::endl; 
      count++;
  }
}
```

当你的Head想传入一个unique_ptr的话，可以使用std::move来进行转移

```cpp
std::unique_ptr<session_t> session = create_session();
request.on_completed([ session = std::move(session),  time = current_time()]
  (response_t response) {
  std::cout << "Got response: " 
            << response << " for session: " 
            << session << " the request took: " 
            << (current_time() - time)<< "milliseconds";
  }
```

**问题: session = std::move(session)时左值还是右值？**

### **3.4 用std :: function包装函数对象**

std::fuction可以很灵活的把一个对象成员函数包装成一个函数，例如我有一个ConnectionPool类IsEmpty函数，

```
ConnectionPool pool;
std::function<bool (ConnectionPool)> f;

f = &ConnectionPool::IsEmpty;
std::cout << f(pool);
```

尽管上述所有因素都使std :: function有用，但不应过度使用它，因为它会带来明显的性能损失。

为了能够隐藏所包含的类型并在所有可调用类型上提供通用接口，std::function使用一种称为类型擦除的技术。我们只需要知道类型擦除通常基于虚拟成员函数调用就足够了。 由于虚拟调用是在运行时解决的，因此编译器无法内联该调用，并且优化机会有限。

std :: function的另一个问题是，尽管其调用运算符被标记为const，但它可以调用non-const可调用对象。 这在多线程代码中可能导致各种问题。

## **Chapter 4**

### **4.1 partial fuction**

批量处理到radians, 三行搞定～

```
std::vector<double> degrees = {0, 30, 45, 60};
std::vector<double> radians(degrees.size());
std::transform(degrees.cbegin(), degrees.cend(), radians.begin(),                           std::bind(std::multiplies<double>(),  PI / 180));
```

std::bind里面有个占位符的概念，可以当形参标记位来用：

```
auto is_greater_than_42 = std::bind(std::greater<double>(), _1, 42);
auto is_less_than_42 = std::bind(std::greater<double>(), 42, _1);
is_less_than_42(6);    // returns true _1 < 42 ?
is_greater_than_42(6); // returns false 42 < _1 ?
```

下面的case中，直接用`_1`代替ga，省得lambda传参～

```
std::vector<Ga> gas;

void print_ga(Ga& ga, std::ostream& out,Ga::format format);

std::for_each(gas.cbegin(), gas.cend(),std::bind(print_ga, _1,                        std::ref(std::cout),  Ga::gaga  ));
```

当然，可以将用文件流代替std::cout，将gaga打印到文件上。如果print_ga是Ga的成员函数，则：

```
std::for_each(gas.cbegin(), gas.cend(),std::bind(&Ga::print_ga, _1,                        std::ref(std::cout),  Ga::gaga  ));
```

Lambdas和std :: bind可为您提供类似水平的表现力，但语法不同。 Lambda往往更冗长，但更可能导致更高效的代码（closure）。

### **4.2 Currying**

自动返回一个函数对象的wrapper，来，上 gaga case

```
bool older_ga(Ga& ga1, Ga& ga2)
{
  return ga1.age > ga2.age;
}

auto curried_older_ga(Ga& ga1)
{
  return [ga1](Ga& ga2){
    return ga1.age > ga2.age;
  };
}

older_ga(ga1, ga2);
curried_older_ga(ga1); // 返回一个函数对象，
curried_older_ga(ga1)(ga2); // 返回bool
```

有lambda推导出返回类型，但这玩意有用吗？我很喜欢book list 4.14的名字：`Solving the API boom with curried functions`,怎么解决API boom呢？

```
results = query(local_connection, session,
          table, filter);                    // 后面两个形参没有用default来省略

auto local_query = query(local_connection);   // 返回的是函数对象
auto remote_query = query(remote_connection); // 这个也是

results = local_query(session, table, filter);

auto main_query = query(local_connection,
                        main_session);
// 如果你第一次调用的时候没加就不能省略table/filter，但第二次调用就可以

results = main_query(table, filter);

auto movies_query = main_query(table);

results = movies_query(filter);
```

秒哇，但我怕写出来有点难懂。当然也可以用blind去生成local_query/remote_query，这样代码更容易读:

```
auto local_query = query(local_connection);
auto local_query = std::bind(query, local_connection, _1, _2, _3);
auto session_query = std::bind(query, _1, main_session, _2, _3);
```

但currying对参数个数更加灵活（不用占位符），想多少个都可以。

### **4.3 Function composition**

1986年，高德纳受Communications of the ACM杂志编程珠玑栏目的邀请写一个字符高频统计的程序，问题定义为：统计出一篇文章中前n个频率最高的单词。高德纳在Pascal上开发了一个解法，共计10页。 包含代码注释，高德纳还在其中定义了用于管理单词计数列表的新颖数据结构。 作为回应，Doug McIlroy编写了一个UNIX shell脚本，解决了相同的问题，但仅用了六行代码：

```
tr -cs A-Za-z '\n' |
    tr A-Z a-z |
    sort |
    uniq -c |
    sort -rn |
    sed ${1}q
```

本质上是一个map-reduce问题，那么，这个用cpp怎么写呢？

```
int most_common_words(const std::string& text)
{
  return print_pairs(
    sort_by_frequency(
      reverse_pairs(
        count_occurrences(
          words(text)
        )
      )
    )
  );
}
```

我的**代码实现**: `most_common_words.cpp`

```cpp
#include <bits/stdc++.h>

std::vector<std::string> words(const std::string& text);
std::unordered_map<std::string, int> count_occurrences(std::vector<std::string> words_set);
template <typename T1, typename T2>
auto reverse_pairs(std::unordered_map<T2, T1> words_map) -> std::vector<std::pair<T1, T2>>;
std::vector<std::pair<int, std::string>> sort_by_frequency(std::vector<std::pair<int, std::string>> words_pair);
int print_pairs(std::vector<std::pair<int, std::string>> sorted_words_pair);

int most_common_words(const std::string& text)
{
  return print_pairs(
    sort_by_frequency(
      reverse_pairs(
        count_occurrences(
          words(text)
        )
      )
    )
  );
}

int main(){ 
  std::string text = "Function composition Back in 1986, the famous Donald Knuth was asked to to to 1986";
  most_common_words(text);
  return 0;
}

std::vector<std::string> words(const std::string& text)
{
  std::regex ws_re("\\s+"); // whitespace
  std::vector<std::string> v(std::sregex_token_iterator(text.begin(), text.end(), ws_re, -1),
    std::sregex_token_iterator());
  return v;
}

std::unordered_map<std::string, int> count_occurrences(std::vector<std::string> words_set)
{
  std::unordered_map<std::string, int> words_map;
  for(auto&& word : words_set)
  {
    if(words_map.find(word)==words_map.end())
      words_map[word] = 1;
    else
      words_map[word]++;
  }
  return words_map;
}

template <typename T1, typename T2>
auto reverse_pairs(std::unordered_map<T2, T1> words_map) -> std::vector<std::pair<T1, T2>> 
{
  std::vector<std::pair<T1, T2>> pairs;
  for(auto item : words_map)
  {
    pairs.push_back(std::make_pair(item.second, item.first));
  }
  return pairs;
}

std::vector<std::pair<int, std::string>>  sort_by_frequency(std::vector<std::pair<int, std::string>> words_pair)
{
  using Pair = std::pair<int, std::string>;
  std::sort(words_pair.begin(), words_pair.end(),[](const Pair &p1, const Pair &p2){return p1.first > p2.first;});
  return words_pair;
}

int print_pairs(std::vector<std::pair<int, std::string>> sorted_words_pair)
{
  for(auto pair : sorted_words_pair)
  {
    std::cout << pair.second << ": " << pair.first << std::endl;
  }
  return 1;
}
```

## **Chapter 5 避免使用mutable**

比如将路网mutable，现实环境中路网的确是mutable的，但对于一个运算过程而已，一直随时可变的路网可能会造成大麻烦。

低层级的并行化代码最大加速比不超过20，如果想在更高层次上加快运算速度，可选择的方法有:

> If lowering the level of parallelism your code can achieve isn’t what you want, you need to find alternative solutions to mutexes for the concurrency problem. Remember that I said the problems appear only if you have mutable data shared across different concurrently running processes. Therefore, one solution is to not have concurrency, and another is not to use mutable data. But there’s also a third option: to have mutable data, but not share it. If you don’t share your data, you’ll never have the problem that someone has changed it without you knowing. You have four options: to have immutable data and not to share it, to have mutable data and not to share it, to have immutable data and to share it, and to have mutable data and to share it.

下述代码，某些编译器优化会失效，造成无法使用移动语义：

```c++
class person_t {
public:
  const std::string name;
  const std::string surname;
  ...
};
```

解决方法是将将public的成员函数设为const：

```
class person_t
{
public:
  std::string name() const;
  std::string surname() const;
private:
  std::string m_name;
  std::string m_surname;
  ...
};
```

**practice** 有很多充电桩，它们公用一套Option，一种做法是写个充电桩manager,另一种做法时仅仅写个map来管理这些充电桩，那么Option怎么传递呢，code it～

## **Chapter 6 Lazy evaluation**

```
template <typename F>
class lazy_val {
private:
  F m_computation; /* 计算过程的函数对象 */
  mutable bool m_cache_initialized; /* 是否计算完的flag */
  mutable decltype(m_computation()) m_cache; /* 缓存计算结果 */
  // 互斥锁阻止多个线程尝试同时初始化缓存。
  mutable std::mutex m_cache_mutex;
public:
  lazy_val(F computation):
    m_computation(computation),
    m_cache_initialized(false) {}
};

template <typename F>
inline lazy_val<F> make_lazy_val(F&& computation) {
  return lazy_val<F>(std::forward<F>(computation));
}

```

对于初始化线程安全，可以使用std::call_once来保证只初始化一次。Laziness：第一次需要时就计算一个值，然后将其缓存以备后用。

### **Chapter 7 range**

在c++写出像sh的代码，依赖于boost.range库，实用性一般。

### **Chapter 8 持久化数据结构**

首先，多个线程贡献一个序列化的数据结构，比如链表。并发的程序在对数据结构的修改时会遇到以下问题：

1. 我们希望对数据的一连串操作是原子的。持久化数据结构能保证你在操作成功之前，将数据原来的状态备份下来，直到成功了才将原来的状态替换掉；

### **Code it**

**使用cpp实现vector trie数据结构**