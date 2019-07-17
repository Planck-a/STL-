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
```cpp
template<typename T>
const T& max(const T&a,const T& b)
{
  return a>b?a:b;
}
```
如果传进来的并不是整数，那么max怎么比较大小？？

方法一：
```cpp
max(string("zoo"),string("hello"));//默认的字符串比较大小，只比较第一个元素大小

bool strlonger(const string&s1,const strig&s2)
{
    return a.size()>b.size();
}
max(string("zoo"),string("hello"),strlonger);
```


方法二：
在特殊的类中重载比较符 <  >
```cpp
class stone
{
    bool operator>(stone s1,stone s2)
    {
      
    }
}
```
7 函数模板、类模板（泛化、偏特化、全特化）
---
(1)、C++模板

说到C++模板特化与偏特化，就不得不简要的先说说C++中的模板。我们都知道，强类型的程序设计迫使我们为逻辑结构相同而具体数据类型不同的对象编写模式一致的代码，而无法抽取其中的共性，这样显然不利于程序的扩充和维护。C++模板就应运而生。C++的模板提供了对逻辑结构相同的数据对象通用行为的定义。这些模板运算对象的类型不是实际的数据类型，而是一种参数化的类型。C++中的模板分为类模板和函数模板。

注意：

编译器并不是把函数模板处理成能够处理任何类型的函数；函数模板通过具体类型产生不同的函数；编译器会对函数模板进行两次编译，在声明处对模板代码本身进行编译（检查语法），在调用处对参数替换后的代码进行编译。

函数模板的实例化是由编译程序在处理函数调用时自动完成的，而类模板的实例化必须有程序员在程序中显式地指定，即 函数模板允许隐式调用和显式调用而类模板只能显示调用。

注意在继承中，类模板的写法
```cpp
template<typename T>
class A{};
 
template<typename T>
class B:public A<typename T>{};


#include <iostream>
using namespace std;
  
template <class T>
class TClass
{
public:
     // TClass的成员函数  
private:
     T DateMember;
};
```

函数模板如下：
```cpp
template <class T>
T Max(const T a, const T b)
{
     return  a > b ? a : b;
}
```
(2)、模板特化

有时为了需要，针对特定的类型，需要对模板进行特化，也就是所谓的特殊处理。比如有以下的一段代码：
```
#include <iostream>
using namespace std;
  
template <class T>
class TClass
{
public:
     bool Equal(const T& arg, const T& arg1);
};
  
template <class T>
bool TClass<T>::Equal(const T& arg, const T& arg1)
{
     return (arg == arg1);
}
  
int main()
{
     TClass<int> obj;
     cout<<obj.Equal(2, 2)<<endl;
     cout<<obj.Equal(2, 4)<<endl;
}
```
类里面就包括一个Equal方法，用来比较两个参数是否相等；上面的代码运行没有任何问题；但是，你有没有想过，在实际开发中是万万不能这样写的，对于float类型或者double的参数，绝对不能直接使用“==”符号进行判断。所以，对于float或者double类型，我们需要进行特殊处理，处理如下：
```
#include <iostream>
using namespace std;
  
template <class T>
class Compare
{
public:
     bool IsEqual(const T& arg, const T& arg1);
};
  
// 已经不具有template的意思了，已经明确为float了
template <>
class Compare<float>
{
public:
     bool IsEqual(const float& arg, const float& arg1);
};
  
// 已经不具有template的意思了，已经明确为double了
template <>
class Compare<double>
{
public:
     bool IsEqual(const double& arg, const double& arg1);
};
  
template <class T>
bool Compare<T>::IsEqual(const T& arg, const T& arg1)
{
     cout<<"Call Compare<T>::IsEqual"<<endl;
     return (arg == arg1);
}
  
bool Compare<float>::IsEqual(const float& arg, const float& arg1)
{
     cout<<"Call Compare<float>::IsEqual"<<endl;
     return (abs(arg - arg1) < 10e-3);
}
  
bool Compare<double>::IsEqual(const double& arg, const double& arg1)
{
     cout<<"Call Compare<double>::IsEqual"<<endl;
     return (abs(arg - arg1) < 10e-6);
}
  
int main()
{
     Compare<int> obj;
     Compare<float> obj1;
     Compare<double> obj2;
     cout<<obj.IsEqual(2, 2)<<endl;
     cout<<obj1.IsEqual(2.003, 2.002)<<endl;
     cout<<obj2.IsEqual(3.000002, 3.0000021)<<endl;
}
```

(3)、模板偏特化

上面对模板的特化进行了总结。那模板的偏特化呢？所谓的偏特化是指提供另一份template定义式，而其本身仍为templatized；也就是说，针对template参数更进一步的条件限制所设计出来的一个特化版本。这种偏特化的应用在STL中是随处可见的。

与模板特化的区别在于，模板特化以后，实际上其本身已经不是templatized，而偏特化，仍然带有templatized。我们来看一个实际的例子：
```
#include <iostream>
using namespace std;
  
// 一般化设计
template <class T, class T1>
class TestClass
{
public:
     TestClass()
     {
          cout<<"T, T1"<<endl;
     }
};
  
// 针对普通指针的偏特化设计
template <class T, class T1>
class TestClass<T*, T1*>
{
public:
     TestClass()
     {
          cout<<"T*, T1*"<<endl;
     }
};
  
// 针对const指针的偏特化设计
template <class T, class T1>
class TestClass<const T*, T1*>
{
public:
     TestClass()
     {
          cout<<"const T*, T1*"<<endl;
     }
};
  
int main()
{
     TestClass<int, char> obj;
     TestClass<int *, char *> obj1;
     TestClass<const int *, char *> obj2;
  
     return 0;
}
```

(4)、特化与偏特化的调用顺序

对于模板、模板的特化和模板的偏特化都存在的情况下，编译器在编译阶段进行匹配时，是如何抉择的呢？从哲学的角度来说，应该先照顾最特殊的，然后才是次特殊的，最后才是最普通的。也就是哪个最优匹配，就选择哪个；如果都不匹配，那就选择通用的。编译器进行抉择也是尊从的这个道理。
