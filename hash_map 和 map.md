1 map 和 hash_map的比较
---
hash_map 和 map的相同点：

  都提供了key-value的存储和查找功能
  
hash_map 和 map的不同点：

  （1）构造的函数不同。hash_map需要hash函数，比较函数；map只需要比较函数(小于函数).
  
  （2）存储结构。hash_map采用hash表存储，占用内存更多；map一般采用红黑树(RB Tree)实现
  
  （3）查找效率。hash_map 查找速度会比map快，选择合适的哈希函数，整个查找效率属于常数级别;而map的查找速度是log(n)级别。
  
 2 hash_map实现原理：
 ---
使用一个下标范围比较大的数组来存储元素。可以设计一个函数（哈希函数，也叫做散列函数），使得每个元素的key都与一个函数值（即 数组下标，hash值）相对应，于是用这个数组单元来存储这个元素；也可以简单的理解为，按照函数值为每一个元素“分类”，然后将这个元素存储在相应 “类”所对应的地方，称为桶。

桶的数目一般选择合适的质数，在SGI STL中这个值已经提前算好了，为{ }；初始时选择47，当某个桶中元素数目多于47时，进行再哈希操作，选择桶数为91，重复上述hash函数计算哈希值并插入过程。


 总的来说，hash_map，首先分配一大片内存（桶数 × sizeof(int * )），形成许多桶。再利用hash函数，对key进行映射到不同区域（桶）进行保存。
 
 其插入过程是：

  1 得到key

  2 通过hash函数得到hash值

  3 得到桶号(一般都为hash值对桶数求模)

  4 存放key和value在桶内。

  其取值过程是:

  1 得到key

  2 通过hash函数得到hash值

  3 得到桶号(一般都为hash值对桶数求模)

  4 比较桶的内部元素是否与key相等，若都不相等，则没有找到。

  5 取出相等的记录的value。

3 STL中 hash_map实现
---
```
#include <hash_map>
#include <string>
using namespace std;
int main(){
        hash_map<int, string> mymap;
        mymap[9527] = "唐伯虎点秋香";
        mymap[1000000] = "百万富翁的生活";
        mymap[10000] = "白领的工资底线";
        ...
        if(mymap.find(10000) != mymap.end()){
                ...
        }
```
使用起来和map是一样的，但是可以自己指定hash函数和比较函数，如果没有指定，编译器也会有默认的函数。下面是SGI STL的声明：
```
template <class _Key, class _Tp, class _HashFcn = hash<_Key>,class _EqualKey = equal_to<_Key>,
class _Alloc = __STL_DEFAULT_ALLOCATOR(_Tp) >
class hash_map
{
        ...
}
```
（1）hash_map 的 hash函数
```
struct hash<int> {
        size_t operator()(int __x) const { return __x; }
};

在SGI STL中，提供了以下hash函数：
struct hash<char*>
struct hash<const char*>
struct hash<char> 
struct hash<unsigned char> 
struct hash<signed char>
struct hash<short>
struct hash<unsigned short> 
struct hash<int> 
struct hash<unsigned int>
struct hash<long> 
struct hash<unsigned long>
```
如果你的key使用的是以上类型中的一种，你都可以使用缺省的hash函数。当然你自己也可以定义自己的hash函数。对于自定义变量，你只能如此，例如对于string，就必须自定义hash函数。
```
struct hash_string{
        size_t operator()(const string& str) const
        {
                unsigned long __h = 0;
                for (size_t i = 0 ; i < str.size() ; i ++)
                __h = 5*__h + str[i];
                return size_t(__h);
        }
};

```
(2)hash_map 的 比较函数

map中的比较函数，需要提供less函数,如果没有提供，缺省的也是less< Key> 。在hash_map中，要比较桶内的数据和key是否相等，因此需要的是是否等于的函数:equal_to< Key> 。
