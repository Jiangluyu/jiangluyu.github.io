---
title: C++set详解
date: 2020/02/27 00:43:00
categories: C++
tags: [算法, STL]
---

关于C++ STL set的用法以及知识延伸。

<!-- more -->

```c++
#include <iostream>
#include <set>

using namespace std;

int main(void)
{
    set<int> nums;
    for (int i = 0; i < 10; i++) {
        nums.insert(i);
    }
    
    // 等效 set<int>::iterator it = nums.begin();
    auto it = nums.begin();
    
    for (it; it != nums.end(); it++) {
        cout << *it << endl;
    }
}
```

### 常见用法

```c++
begin() // 返回指向set容器第一个元素的迭代器

end() // 返回指向set容器最后一个元素的迭代器，不存值
    
rbegin() // 等效end()
    
rend() // 等效begin()
    
clear() // 清空set容器
    
empty() // 判断set容器是否为空
    
max_size() // 返回set容器可能包含元素的最大个数
    
size() // 返回当前set容器中的元素个数
    
insert(x) // 向set容器中插入值x，返回值为pair<it, bool>，bool表示是否成功，it指向x在set中的位置
    
insert(it_start, it_end) // 向set中插入两迭代器中间的值
    
erase(x) // 从set容器中删除值x，若x为迭代器，则删除迭代器指向的值
    
erase(it_start, it_end) // 删除set中两迭代器中间的值，包括*it_start，不包括*it_end
    
count(x) // 计算set中x出现的次数，在set中只可能为1或0
    
find(x) // 若找到x，则返回指向x的迭代器，否则返回end()
    
lower_bound(x) // 返回指向第一个大于等于x的值的迭代器
    
upper_bound(x) // 返回指向第一个大于x的值的迭代器

// 返回pair<it, it>，即在两迭代器内，所有等于x的值的范围，即第一个x的位置与最后一个x的位置，set中若不存在x则两值相等
equal_range(it_begin, it_end, x) 

set1.insert(set2.begin(), set3.end()) // set1 = set2 ∪ set3 并去重

set1.swap(set2) // 两个set内容交换
```

### 知识延伸

##### map和set插入删除效率比其他序列容器高的原因：

> 底层实现为红黑树，对于关联容器来说，不需要做内存拷贝和内存移动。set容器内所有元素都是以节点的方式来存储，其节点结构和链表差不多，指向父节点和子节点。

##### map在每次insert、erase操作之后，以前的迭代器是否会过期？

> 不会，注意vector在进行相应操作后迭代器可能会过期。

##### 插入、搜索元素时，效率如何？

> set中使用的是二分查找，时间复杂度为*logn*，换言之，数据增大一倍时，搜索次数会多1次。

##### 如何求并集、交集、差集、对称差集（A∪B-A∩B）？

> ```c++
> #include <algorithm>
> 
> // 交集
> set_intersection(it1_start, it1_end, it2_start, it2_end, inserter(res, res.begin()))
> 
> // 差集
> set_difference(it1_start, it1_end, it2_start, it2_end, inserter(res, res.begin()))
> 
> // 并集
> set_union(it1_start, it1_end, it2_start, it2_end, inserter(res, res.begin()))
>     
> // 对称差集
> set_symmetry_difference(it1_start, it1_end, it2_start, it2_end, inserter(res, res.begin()))
> ```