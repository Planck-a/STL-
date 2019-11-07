[1](#https://github.com/Planck-a/image-folder/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/vector.jpg)

1、vector的内部实现
---
vector内部成员变量有三个，start、finish和end_of_storage，所以vector在32位平台下大小为3*4=12字节。

T* ------->iterator------>iterator start;     iterator finish;      iterator end_of_storage;迭代器就是指针

size()函数计算的就是指针finish-start，指针间距就是大小

同理，capacity()返回的即使end_of_storage-start。

2、两倍增长：
---
答：push_back()函数，先判断finish==end_of_storage？，如果已无可用空间了，则调用insert_aux()函数，无可用空间有三种情况：

1> 大小为0，则返回长度1

2> 原大小n不够用了，申请一个新的2n大小空间，拷贝并在finish+1处插入新元素；并且析构掉旧的内存空间。

3 vector使用的注意点及其原因，频繁对vector调用push_back()对性能的影响和原因。
---

4.vector通过迭代器删除
---
```
for(auto iter = vecInt.begin(); iter != vecInt.end(); iter++
  if(*iter == 444){		
         iter = vecInt.erase(iter);
         iter--;			
     }
	}
```
在使用vector.erase(iterator)的时候需要时刻注意:

如果是444就执行vecInt.erase(iter)。此时iter迭代器被erase，iter指向了一个不确定的地址成为野指针，在for循环的条件中，直接iter++会导致程序崩溃。

但是这个语句执行完会返回下一迭代器位置，用一个迭代器来接这个位置就不会出错。使用 iterator = vector.erase(iterator) 防止变成野指针程序崩溃。同时，由于会自动++，为了遍历所有的数，主动--
