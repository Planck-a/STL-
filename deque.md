deque
---
1 实现：核心的控制中心map 是用vector实现的，所以可以扩容。vector的每个结点指向一个buffer，buffer在之前的版本可以自己设置大小，在新版本默认为512字节，buffer中真实存放的是数据。buffer的迭代器包含四个指针，cur、first、last和map_pointer，并且在迭代器中重载++，--，-，+=操作符，使得deque可以模拟连续空间，实际上是连续的分段空间，deque可以模拟连续空间都是deque_iterator的功劳。

迭代器++，当到达buffer的边界时，会自动跳转到控制中心并查找出下一个buffer的起始位置
