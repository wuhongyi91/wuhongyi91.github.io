---
layout: post
title: "C++ map的基本操作和用法"
date: 2013-12-25 19:17:00 +0800
comments: true
tags: [Linux,C/C++]
---

## 1、map简介
map 是一类关联式容器。它的特点是增加和删除节点对迭代器的影响很小，除了那个操作节点，对其他的节点都没有什么影响。对于迭代器来说，可以修改实值，而不能修改 key。<!--more-->
## 2、map的功能
自动建立 Key－value 的对应。key 和 value 可以是任意你需要的类型。
根据 key 值快速查找记录，查找的复杂度基本是 Log(N)，如果有 1000 个记录，最多查找 10 次，1,000,000个记录，最多查找 20 次。
- 快速插入 Key-Value 记录;
- 快速删除记录;
- 根据Key 修改value记录;
- 遍历所有记录。
## 3、使用map
使用 map 得包含 map 类所在的头文件  
~~~C++
#include <map> //注意，STL头文件没有扩展名.h
~~~

map 对象是模板类，需要关键字和存储对象两个模板参数：
~~~C++
std:map<int, string> personnel;
~~~

这样就定义了一个用 int 作为索引,并拥有相关联的指向 string 的指针。

为了使用方便，可以对模板类进行一下类型定义，
~~~C++
typedef map<int, CString> UDT_MAP_INT_CSTRING;
UDT_MAP_INT_CSTRING enumMap;
~~~

## 4、在map中插入元素
改变 map 中的条目非常简单，因为 map类已经对 [] 操作符进行了重载
~~~C++
enumMap[1] = "One";
enumMap[2] = "Two";
//.....
~~~

这样非常直观，但存在一个性能的问题。插入 2 时,先在 enumMap 中查找主键为 2 的项，没发现，然后将一个新的对象插入 enumMap，键是 2，值是一个空字符串，插入完成后，将字符串赋为 "Two"; 该方法会将每个值都赋为缺省值，然后再赋为显示的值，如果元素是类对象，则开销比较大。我们可以用以下方法来避免开销：
~~~C++
enumMap.insert(map<int, CString> :: value_type(2, "Two"))
~~~
## 5、查找并获取map中的元素
下标操作符给出了获得一个值的最简单方法：
~~~C++
//CString 只适用于windows
CString tmp = enumMap[2];
~~~
但是,只有当 map 中有这个键的实例时才对，否则会自动插入一个实例，值为初始化值。

我们可以使用 Find() 和 Count() 方法来发现一个键是否存在。

查找 map 中是否包含某个关键字条目用 find() 方法，传入的参数是要查找的 key，在这里需要提到的是 begin() 和 end() 两个成员，分别代表 map 对象中第一个条目和最后一个条目，这两个数据的类型是 iterator。
~~~C++
int nFindKey = 2; //要查找的Key
//定义一个条目变量(实际是指针)
UDT_MAP_INT_CSTRING::iterator it= enumMap.find(nFindKey);
if(it == enumMap.end()) {
//没找到
}
else {
//找到
}
~~~
通过 map 对象的方法获取的 iterator 数据类型是一个 std::pair 对象，包括两个数据 iterator->first 和 iterator->second 分别代表关键字和存储的数据。
## 6、从map中删除元素
移除某个 map 中某个条目用 erase()。  
该成员方法的定义如下：
~~~C++
iterator erase(iterator it); //通过一个条目对象删除
iterator erase(iterator first, iterator last); //删除一个范围
size_type erase(const Key& key); //通过关键字删除
clear()就相当于 enumMap.erase(enumMap.begin(), enumMap.end());
~~~

----

**C++ STL map的使用**

以下是对C++中STL map的插入，查找，遍历及删除的例子：
~~~C++
#include <map>
#include <string>
#include <iostream>
using namespace std;

void map_insert(map < string, string > *mapStudent, string index, string x)
{
mapStudent->insert(map < string, string >::value_type(index, x));
}

int main(int argc, char **argv)
{
char tmp[32] = "";
map < string, string > mapS;

//insert element
map_insert(&mapS, "192.168.0.128", "xiong");
map_insert(&mapS, "192.168.200.3", "feng");
map_insert(&mapS, "192.168.200.33", "xiongfeng");

map < string, string >::iterator iter;

cout << "We Have Third Element:" << endl;
cout << "-----------------------------" << endl;

//find element
iter = mapS.find("192.168.0.33");
if (iter != mapS.end()) {
cout << "find the elememt" << endl;
cout << "It is:" << iter->second << endl;
} else {
cout << "not find the element" << endl;
}

//see element
for (iter = mapS.begin(); iter != mapS.end(); iter ) {

cout << "| " << iter->first << " | " << iter->
second << " |" << endl;

}
cout << "-----------------------------" << endl;

map_insert(&mapS, "192.168.30.23", "xf");

cout << "After We Insert One Element:" << endl;
cout << "-----------------------------" << endl;
for (iter = mapS.begin(); iter != mapS.end(); iter ) {

cout << "| " << iter->first << " | " << iter->
second << " |" << endl;
}

cout << "-----------------------------" << endl;

//delete element
iter = mapS.find("192.168.200.33");
if (iter != mapS.end()) {
cout << "find the element:" << iter->first << endl;
cout << "delete element:" << iter->first << endl;
cout << "=================================" << endl;
mapS.erase(iter);
} else {
cout << "not find the element" << endl;
}
for (iter = mapS.begin(); iter != mapS.end(); iter ) {

cout << "| " << iter->first << " | " << iter->
second << " |" << endl;

}
cout << "=================================" << endl;

return 0;
}
~~~

----
本文转载自：http://blog.csdn.net/xie376450483/article/details/5804974 
