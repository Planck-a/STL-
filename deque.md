deque
---
1 deque的实现：
---
核心的控制中心map 是用vector实现的，所以可以扩容。vector的每个结点指向一个buffer，buffer在之前的版本可以自己设置大小，在新版本默认为512字节，buffer中存放的是真实数据。buffer的迭代器包含四个指针，cur、first、last和map_pointer，并且在迭代器中重载++，--，-，+=操作符，使得deque可以模拟连续空间，实际上是连续的分段空间，deque可以模拟连续空间都是deque_iterator的功劳。

迭代器++，当到达buffer的边界时，会自动跳转到控制中心并查找出下一个buffer的起始位置

```cpp
template<typename T>
{
protected:
    iterator start;   //16字节，迭代器由4个字节组成
    iterator finish;
    size_type map_size;//T  4字节
    map_pointer map;//T**  4字节
}

class iterator   //大小12字节
{
    T* cur;
    T* first;
    T* last;
    map_pointer map;
}
```
2 deque和queue/stack的关联
---
queue和stack
内部都是包含了一个deque，只不过是封住了deque的某些接口；（此外这两个也可以用list做底层支撑）；

容器适配器：stack,queue,priority_queue不允许遍历、插入，没有迭代器！！

stack可以用vector做底部支撑；
queue不能用vector做底部支撑，因为vector没有pop()函数
