#### 1. GCL 

src: 

a tiny graph concurrent library



基本的流程：

record tasks, schedule, executive.

核心的接口：`schedule_all`:

```c++
template<typename Result>
bool BaseTask<Result>::schedule_all(gcl::Exec& exec)
{
    if (is_scheduled()) // 
    {
        return false;
    }
    if (exec.n_threads() == 0)
    {
        return schedule_all();
    }
    std::vector<gcl::ITask*> roots; // 找出taskflow的根节点
    m_impl->visit([&roots](BaseImpl& i) // visit： 回溯， m_task_cache
    {
        i.prepare(); // 设置已规划的flag，is_scheduled() 为 true
        if (i.parents().empty())
        {
            roots.emplace_back(&i);
        }
    });
    for (const auto root : roots)
    {
        exec.execute(*root);
    }
    return true;
}
```



#### folly: concurrent hash map



ref: https://zhuanlan.zhihu.com/p/104308755



insert 操作如何确保线程安全？

```c++
  template <typename Key>
  std::pair<ConstIterator, bool> insertImpl(std::pair<Key, mapped_type>&& foo) {
    auto segment = pickSegment(foo.first);
    std::pair<ConstIterator, bool> res(
        std::piecewise_construct,
        std::forward_as_tuple(this, segment),
        std::forward_as_tuple(false));
    res.second = ensureSegment(segment)->insert(res.first.it_, std::move(foo));
    return res;
  }
```

核心的impl为：

```c++
class alignas(64) BucketTable {
```

alignas关键字明确指定占用字节的大小，跨平台用的。

```c++
  template <typename MatchFunc, typename K, typename... Args>
  bool doInsert(
```



## cpp-utility

https://github1s.com/eteran/cpp-utilities



#### string



