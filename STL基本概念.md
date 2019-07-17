1 STL六大部件
--
容器  算法   迭代器   分配器    适配器Adapters    仿函数Functors

2 常用容器
---
顺序容器：vector、array、list、forward-list、queue、stack、deque

关联容器：set、map、multiset、multimap、unorderedmap、unorderedset

关联式容器是说存入的元素按照key-value对应，且按照key进行排序，map/set/multiset/multimap底层为红黑树,unorderedmap/unorderedset底层为hashtable。
set就是特殊的map，其key就是value，value就是key

3、hash_table
---
解决hash冲突的方法很多，STL标准库中都是用拉链法解决的。但是链表的查找效率很低为O(n),每个拉链都不能太长，如果拉链太长的话会怎样处理呢？？

4、测试程序的小技巧
---
（1）用namespace分开各个部分
```cpp
#include <stdio.h>
#include<vector>
namespace gg01
{
  ...
}

#include <stdio.h>
#include<string>
namespace gg02
{
  ...
}

```
(2)变量使用时再定义，不要放到最开头一起定义；定义变量时可以顶头，去掉缩进，这样在程序中比较直观。

5、关于算法sort(a.begin(),a.end())
---
（1）为什么list不能用sort(a.begin(),a.end())？

答：sort在实现里有一句核心代码，*(first+(last-first)/2)，而list是不支持下标操作的，所以不能排序；但是STL的list中自己写了sort()函数，不需要通过迭代器对容器中的数进行排序。

6、max算法
---

