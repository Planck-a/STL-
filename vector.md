[vector内部图](https://github.com/Planck-a/image-folder/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/vector.jpg)

1、vector的内部实现
---
vector内部成员变量有三个，start、finish和end_of_storage，所以vector在32位平台下大小为3*4=12字节。

T* ------->iterator------>iterator start;     iterator finish;      iterator end_of_storage;迭代器就是指针

size()函数计算的就是指针finish-start，指针间距就是大小

同理，capacity()返回的即使end_of_storage-start。

vector扩容(两倍增长)
---
答：push_back()函数，先判断finish==end_of_storage？，如果已无可用空间了，则调用insert_aux()函数，无可用空间有三种情况：

1> 大小为0，则返回长度1

2> 原大小n不够用了，申请一个新的2n大小空间，拷贝并在finish+1处插入新元素；并且析构掉旧的内存空间。

3 vector使用的注意点及其原因，频繁对vector调用push_back()对性能的影响和原因。


## 迭代器失效

### vector的迭代器失效
* **插入元素**
  * 插入一个元素后，end操作返回的迭代器肯定失效
  * 插入一个元素但是capacity没有变，那么插入点之前的迭代器有效，之后的iterator会失效
  * 插入元素导致capacity不够，此时first和end的操作返回的迭代器都会失效
* **删除元素**
  * 删除点之前的迭代器有效，指向删除点及以后的全部迭代器失效
  
错误做法：  
```
while (it != v.end())
    {
        if (*it % 2 == 0)
        {
            it = v.erase(it);//错误！！
            ++it;
        }
        
    }
```
正确的做法：
```
while (it != v.end())
    {
        if (*it % 2 == 0)
        {
            it = v.erase(it);//直接返回删除点之后节点的位置，不需要++
        }
        else
        {
            ++it;
        }        
    }
```

