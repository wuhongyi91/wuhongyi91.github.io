---
layout: post
title: "C++中头文件（.h）和源文件（.cpp）都应该写些什么 "
date: 2013-08-13 18:18:00 +0800
comments: true
tags: [Linux,C/C++]
---

&#160; &#160; &#160; &#160;在 C++ 编程过程中，随着项目的越来越大，代码也会越来越多，并且难以管理和分析。于是，在 C++ 中就要分出了头(.h)文件和实现(.cpp)文件。
<!--more-->
## 头文件(.h)
**写类的声明（包括类里面的成员和方法的声明）、函数原型、#define常数等，但一般来说不写出具体的实现。**  
在写头文件时需要注意，在开头和结尾处必须按照如下样式加上预编译语句（如下）：
~~~C++
#ifndef CIRCLE_H
#define CIRCLE_H

//你的代码写在这里

#endif
~~~
这样做是为了防止重复编译，不这样做就有可能出错。至 于CIRCLE_H 这个名字实际上是无所谓的（习惯性写类名），你叫什么都行，只要符合规范都行。原则上来说，非常建议把它写成这种形式，因为比较容易和头文件的名字对应。

## 源文件（.cpp）
**源文件主要写实现头文件中已经声明的那些函数的具体代码。需要注意的是，开头必须\#include一下实现的头文件，以及要用到的头文件。**那么当你需要用到自己写的头文件中的类时，只需要#include进来就行了。

下面举个最简单的例子来描述一下，咱就求个圆面积。

- 第1步，建立一个空文件夹（以 g++ 编译为例）。  
- 第2步，在文件夹里新建一个名为 Circle.h 的头文件，它的内容如下：
~~~C++
#ifndef CIRCLE_H
#define CIRCLE_H

class Circle
{
private:
	double r;//半径
public:
	Circle();//构造函数
	Circle(double R);//构造函数
	double Area();//求面积函数
};

#endif
~~~
注意到开头结尾的预编译语句。在头文件里，并不写出函数的具体实现。

- 第3步，要给出Circle类的具体实现，因此，在文件夹里新建一个 Circle.cpp 的文件，它的内容如下：
~~~C++
#include "Circle.h"

Circle::Circle()
{
    this->r=5.0;
}

Circle::Circle(double R)
{
    this->r=R;
}

double Circle:: Area()
{
    return 3.14*r*r;
}
~~~
需要注意的是：开头处包含了 Circle.h，事实上，只要此cpp文件用到的文件，都要包含进来！这个文件的名字其实不一定要叫 Circle.cpp，但非常建议 cpp 文件与头文件相对应。
- 最后，我们建一个 main.cpp 来测试我们写的 Circle类，它的内容如下：
~~~C++
#include <iostream>
#include "Circle.h"
#include "Circle.cpp"
using namespace std;

int main()
{
    Circle c(3);
    cout<<"Area="<<c.Area()<<endl;
    return 0;
}
~~~
## 编译运行
在该文件夹中打开终端：
~~~shell
g++ main.cpp -o 123
./123
~~~
以上就是编译、执行命令，终端中应该输出
Area=28.26
<br />
说明我们写的Circle类确实可以用了。


