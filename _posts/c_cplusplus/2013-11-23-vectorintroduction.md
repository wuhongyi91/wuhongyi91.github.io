---
layout: post
title: "std::vector介绍"
date: 2013-11-23 21:33:00 +0800
comments: true
tags: [Linux,Windows,C/C++,vector]
---

&#160; &#160; &#160; &#160;vector 是 C++ 标准模板库中的部分内容，它是一个多功能的，能够操作多种数据结构和算法的模板类和函数库。vector 之所以被认为是一个容器，是因为它能够像容器一样存放各种类型的对象，简单地说，vector 是一个能够存放任意类型的动态数组，能够增加和压缩数据。<!--more-->
为了可以使用vector，必须在你的头文件中包含下面的代码：
~~~C++
#include <vector>
~~~
vector 属于 **std** 命名域的，因此需要通过命名限定，如下完成你的代码：
~~~C++
using std::vector;
vector<int> vInts;
~~~
或者连在一起，使用全名：
~~~C++
std::vector<int> vInts;
~~~

**建议使用全局的命名域方式：using namespace std;**  

| 函数 | 表述 |
| :--------- | :--------- |
| c.assign(beg,end) | 将 [beg; end) 区间中的数据赋值给 c。 |
| c.assign(n,elem) | 将 n 个 elem 的拷贝赋值给c。 |
| c.at(idx) | 传回索引 idx 所指的数据，如果 idx 越界，抛出 out_of_range。 |
| c.back() | 传回最后一个数据，不检查这个数据是否存在。 |
| c.begin() | 传回迭代器中的第一个数据地址。 |
| c.capacity() | 返回容器中数据个数。 |
| c.clear() | 移除容器中所有数据。 | 
| c.empty() | 判断容器是否为空。 |
| c.end() | 指向迭代器中的最后一个数据地址。 |
| c.erase(pos) | 删除 pos 位置的数据，传回下一个数据的位置。 |
| c.erase(beg,end) | 删除 [beg,end) 区间的数据，传回下一个数据的位置。 |
| c.front() | 传回第一个数据。 |
| get_allocator | 使用构造函数返回一个拷贝。 |
| c.insert(pos,elem) | 在 pos 位置插入一个 elem 拷贝，传回新数据位置。 |
| c.insert(pos,n,elem) | 在 pos 位置插入 n 个 elem 数据。无返回值。 |
| c.insert(pos,beg,end) | 在 pos 位置插入在 [beg,end) 区间的数据。无返回值。 |
| c.max_size() | 返回容器中最大数据的数量。 | 
| c.pop_back() | 删除最后一个数据。 |
| c.push_back(elem) | 在尾部加入一个数据。 |
| c.rbegin() | 传回一个逆向队列的第一个数据。 |
| c.rend() | 传回一个逆向队列的最后一个数据的下一个位置。 |
| c.resize(num) | 重新指定队列的长度。 |
| c.reserve() | 保留适当的容量。 |
| c.size() | 返回容器中实际数据的个数。 |
| c1.swap(c2) | 将 c1 和 c2 元素互换。 |
| swap(c1,c2) | 同上操作。 |
| vector<Elem> c | 创建一个空的 vector。 |
| vector<Elem> c1(c2) | 复制一个 vector。 |
| vector <Elem> c(n) | 创建一个 vector，含有n个数据，数据均已缺省构造产生。 |
| vector <Elem> c(n, elem) | 创建一个含有 n 个 elem 拷贝的 vector。 |
| vector <Elem> c(beg,end) | 创建一个以 [beg;end) 区间的 vector。 |
| c.~ vector <Elem>() | 销毁所有数据，释放内存。 |
| operator[] | 返回容器中指定位置的一个引用。 |

## 创建一个vector
vector 容器提供了多种创建方法，下面介绍几种常用的。
创建一个 Widget类型的空的 vector 对象：
~~~C++
vector<Widget> vWidgets;
~~~

创建一个包含 500 个 Widget类型数据的 vector：
~~~C++
vector<Widget> vWidgets(500);
~~~

创建一个包含 500 个 Widget类型数据的 vector，并且都初始化为 0：
~~~C++
vector<Widget> vWidgets(500, Widget(0));
~~~

创建一个 Widget的拷贝：
~~~C++
vector<Widget> vWidgetsFromAnother(vWidgets);
~~~

## 向 vector 添加一个数据
&#160; &#160; &#160; &#160;vector 添加数据的缺省方法是 push_back()。push_back() 函数表示将数据添加到 vector 的尾部，并按需要来分配内存。例如：向 vector<Widget>中添加 10 个数据，需要如下编写代码：
~~~C++
for(int i= 0;i<10; i++)
    vWidgets.push_back(Widget(i));
~~~

## 获取vector中制定位置的数据
&#160; &#160; &#160; &#160;vector 里面的数据是动态分配的，使用 push_back() 的一系列分配空间常常决定于文件或一些数据源。如果想知道 vector 存放了多少数据，可以使用 empty()。获取 vector 的大小，可以使用 size()。例如，如果想获取一个 vector v 的大小，但不知道它是否为空，或者已经包含了数据，如果为空想设置为 -1，你可以使用下面的代码实现：
~~~C++
int nSize = v.empty() ? -1 : static_cast<int>(v.size());
~~~

## 访问vector中的数据
使用两种方法来访问 vector。
	1、   vector::at()
	2、   vector::operator[]  
operator[] 主要是为了与 C 语言进行兼容。它可以像C语言数组一样操作。但 at() 是我们的首选，因为 at()  进行了边界检查，如果访问超过了 vector 的范围，将抛出一个例外。由于 operator[] 容易造成一些错误，所有我们很少用它，下面进行验证一下：
分析下面的代码：
~~~C++
vector<int> v;
v.reserve(10);
 
for(int i=0; i<7; i++)
    v.push_back(i);
 
try
{
 int iVal1 = v[7]; // not bounds checked - will not throw
 int iVal2 = v.at(7); // bounds checked - will throw if out of range
}
catch(const exception& e)
{
 cout << e.what();
}
~~~

## 删除vector中的数据
&#160; &#160; &#160; &#160;vector 能够非常容易地添加数据，也能很方便地取出数据，同样 vector 提供了 erase()，pop_back()，clear() 来删除数据，当删除数据时，应该知道要删除尾部的数据，或者是删除所有数据，还是个别的数据。  
**Remove_if()算法** 如果要使用 remove_if()，需要在头文件中包含如下代码：
~~~C++
#include <algorithm>
~~~
Remove_if()有三个参数：
- 1、   iterator _First：指向第一个数据的迭代指针。
- 2、   iterator _Last：指向最后一个数据的迭代指针。
- 3、   predicate _Pred：一个可以对迭代操作的条件函数。
**条件函数** 条件函数是一个按照用户定义的条件返回是或否的结果，是最基本的函数指针，或是一个函数对象。这个函数对象需要支持所有的函数调用操作，重载 operator()() 操作。remove_if() 是通过 unary_function 继承下来的，允许传递数据作为条件。  
&#160; &#160; &#160; &#160;例如，假如想从一个 vector<CString> 中删除匹配的数据，如果字串中包含了一个值，从这个值开始，从这个值结束。首先应该建立一个数据结构来包含这些数据，类似代码如下：
~~~C++
#include <functional>
enum findmodes
{
 FM_INVALID = 0,
 FM_IS,
 FM_STARTSWITH,
 FM_ENDSWITH,
 FM_CONTAINS
};
typedefstruct tagFindStr
{
 UINT iMode;
 CString szMatchStr;
} FindStr;
typedef FindStr* LPFINDSTR;
~~~
然后处理条件判断：
~~~C++
class FindMatchingString
    : public std::unary_function<CString, bool>
{
public:
 FindMatchingString(const LPFINDSTR lpFS) : m_lpFS(lpFS) {}
   
 bool operator()(CString& szStringToCompare) const
 {
     bool retVal = false;
 
     switch(m_lpFS->iMode)
     {
     case FM_IS:
       {
         retVal = (szStringToCompare == m_lpFDD->szMatchStr);
         break;
       }
     case FM_STARTSWITH:
       {
         retVal = (szStringToCompare.Left(m_lpFDD->szMatchStr.GetLength())
               == m_lpFDD->szWindowTitle);
         break;
       }
     case FM_ENDSWITH:
       {
         retVal = (szStringToCompare.Right(m_lpFDD->szMatchStr.GetLength())
               == m_lpFDD->szMatchStr);
         break;
       }
     case FM_CONTAINS:
       {
         retVal = (szStringToCompare.Find(m_lpFDD->szMatchStr) != -1);
         break;
       }
     }
       
     return retVal;
 }
       
private:
    LPFINDSTR m_lpFS;
};
~~~
通过这个操作你可以从vector中有效地删除数据：
~~~C++
FindStr fs;
fs.iMode = FM_CONTAINS;
fs.szMatchStr = szRemove;
 
vs.erase(std::remove_if(vs.begin(), vs.end(), FindMatchingString(&fs)), vs.end());
~~~
Remove(),remove_if() 等所有的移出操作都是建立在一个迭代范围上的，不能操作容器中的数据。所以在使用 remove_if()，实际上操作的时容器里数据的上面的。
 
看到 remove_if() 实际上是根据条件对迭代地址进行了修改，在数据的后面存在一些残余的数据，那些需要删除的数据。剩下的数据的位置可能不是原来的数据，但他们是不知道的。
调用erase()来删除那些残余的数据。注意上面例子中通过 erase() 删除 remove_if() 的结果和 vs.enc()  范围的数据。

本文转载自CSDN博客：http://blog.csdn.net/willoj/archive/2008/04/05/2252543.aspx

<br />
----
&#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160; &#160;**Update: 1. Last-Modified: 2013-11-23 21:33**
