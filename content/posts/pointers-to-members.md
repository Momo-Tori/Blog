---
title: "类成员函数指针传参问题"
date: 2021-12-31T11:52:07+08:00
tags: ["ModernCpp","CppClass","Cpp"]
categories: ["Cpp学习笔记"]
---
![Eris](https://pic1.zhimg.com/v2-2ea2c7e8ef116d4aa8830ff1a9ce4922_1440w.jpg?source=172ae18b)

害人不浅的类成员函数指针，模拟电梯做了这么久它功不可没

<!--more-->

## 问题提出

事情起因很长。

期中前后我就写了数据结构的lab1模拟电梯程序，然后带着它去通过了助教测试。在经历了ICS课程的洗礼的几个月后，我已经变成了Linux的形状，命令行都是用WSL2的BASH来输入。但我发现原本在windows下能够编译成功的程序反而编译失败了

让我们随便简单还原一个报错现场

```cpp
#include <iostream>
class temp{
    public:
    void foo(){std::cout<<'?'<<std::endl;}
};

class c{
    public:
    void fun(void (temp::*pFunction)());//pFunction为指向temp的类函数的指针
};
void c::fun(void (temp::*pFunction)()){
    temp a;
    (a.*pFunction)();//简单的调用
}
int main()
{
    c d;
    d.fun(temp::foo);
    return 0;
}
```

在windows下可以编译成功并正常运行，而在Linux下则会出现下面报错

```Bash
error: invalid use of non-static member function
```

问题出在编译器不认可传递参数是non-static的类成员函数指针，也就是代码块中的`d.fun(temp::foo);`

暂且把编译器版本号放在下面：
* Linux：g++ (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
* Windows：g++.exe (x86_64-posix-seh-rev0, Built by MinGW-W64 

这种错误肯定是个大新闻的啊，于是立马进行一个歌的谷，首先得到的结论是报错才是符合ISOCPP的标准的，并且ISOCPP在官网Wiki上还特地有一个相关的[FAQ](https://isocpp.org/wiki/faq/pointers-to-members)

当我怀疑这是MinGW64-GCC新出的bug的时候，我发现早在六年前就有人提出[这个bug](https://stackoverflow.com/questions/33387462/pass-pointer-to-member-function-compiles-in-mingw-w64-but-not-in-gcc)了，也就是说直到现在这个bug还没有被修复，只能说\#查询MinGW-w64的Contributors的工作效率（大雾

## 解决方法

### 使用&与->\*或.\*结构

如果调用函数是用户自己定义的函数，那么我们可以使用`&`创建一个指向“类成员函数指针”的指针，然后在调用函数时使用`类指针->*p(/* args */)`或`类对象.*p(/* args */)`进行解引用即可（注意：解引用后为函数符，仍然需要后面加上括号和参数进行调用）

一开始的例子中只需要将`d.fun(temp::foo);`改为`d.fun(&temp::foo);`即可

此时`&temp::foo`是指向“类成员函数指针”的指针，`*(&temp::foo)`是类成员函数指针也就是一个函数符，为了给这个函数符指明它对应的类对象，需要在前面加上对应的类对象或指针，分别为`a.*(&temp::foo)`或`p->*(&temp::foo)`

但使用->\*或.\*调用函数的方法ISOCPP因为可读性的原因并不推荐，而是推荐使用下面的方法


### 使用std::invoke

std::invoke是定义在头文件\<functional\>，需要C++17特性的函数

参数：
invoke( F&& f, Args&&... args )
f	-	要调用的可调用 (Callable) 对象
args	-	传递给 f 的参数

返回值：
f 返回的值

>The operation INVOKE(f, t1, t2, ..., tN) is defined as follows:
> * If f is a pointer to member function of class T:
If std::is_base_of<T, std::decay_t<decltype(t1)>>::value is true, then INVOKE(f, t1, t2, ..., tN) is equivalent to (t1.*f)(t2, ..., tN)

>来自[https://en.cppreference.com/w/cpp/utility/functional/invoke](https://en.cppreference.com/w/cpp/utility/functional/invoke)，有删减

这里是第一种假设的第一种情况，也就是说当f是指向“类成员函数指针”的指针，第一个参数t1是要使用函数的类，那么std::invoke(f, t1, t2, ..., tN)就等价于(t1.*f)(t2, ..., tN)，这样就实现了函数的调用

注：编译选项需要`-std=c++17`或更高

  

但如果需要传参的函数不是用户自己定义的，不能使用上面对应的结构，比如说std::sort参数中的compare函数，那应该怎么办呢？

### 定义一个非成员函数进行传参

这是上面的[FAQ](https://isocpp.org/wiki/faq/pointers-to-members)所给出的方法
> As a patch for existing software, use a top-level (non-member) function as a wrapper which takes an object obtained through some other technique.

原文也给出了例子

```cpp
class Fred {
public:
  void memberFn();
  static void staticMemberFn();  // A static member function can usually handle it
  // ...
};
// Wrapper function uses a global to remember the object:
Fred* object_which_will_handle_signal;
void Fred_memberFn_wrapper()
{
  object_which_will_handle_signal->memberFn();
}
int main()
{
  /* signal(SIGINT, Fred::memberFn); */   // Can NOT do this
  signal(SIGINT, Fred_memberFn_wrapper);  // Okay
  signal(SIGINT, Fred::staticMemberFn);   // Okay usually; see below
  // ...
}
```

##### Note
上面的FAQ还提到许多Can or Can't，在这里特别摘录一个Q&A

* 不要使用强制类型转换为`void *`类型的指针：
    因为指向成员函数的指针可能是一个数据结构而非普通的指针

### 使用std::bind

摸大鱼咯

扔个网址就润

[https://zhuanlan.zhihu.com/p/366654169](https://zhuanlan.zhihu.com/p/366654169)

### 使用函数类

所谓函数类的主要思想就是通过重载类的`operator()`，使得一个类起到函数的作用

下面的例子是通过将传递类函数指针转化为传递函数类，并将原本类的this指针作为参数初始化函数类

```cpp
class MyClass {
    struct Less {
        Less(const MyClass& c) : myClass(c) {}
        bool operator () ( const int & i1, const int & i2 ) {// use 'myClass'} 
        MyClass& myClass;
    };
    doSort() { std::sort(arr, arr+someSize, Less(*this)); }
}
```

> 摘自[https://stackoverflow.com/a/1902321](https://stackoverflow.com/a/1902321)

此时传递进去的是一个类对象，但是这个对象可以在后面加上括号后调用函数，和一个函数名或函数指针没有区别

### 使用lambda函数

同上，最重要的一部分就是能够将this指针传入函数，下面是一个例子

```cpp
class Myclass {
    public:
        bool cmp (const int& a, cont int& b)  return a > b;
        void foo() {
           sort(nums.begin(), nums.end(), 
               [&] (const int& a, const int& b) {  //或者按值捕获  [=]
//更直观一些，让lambda函数捕获this指针 [this] (const int& a, const int& b){
                       return cmp(a, b);
                   });
        }
        std::vector<int> nums = {1,4,5,3,7,6,2};
}
```
> 因为通过按值捕获或按引用捕获，lambda可以获取到外部那个看不见的this指针，因此在添加捕获设置之后，调用cmp相当于是在调用this->cmp，自然也可以通过编译。这种方式确实要比std::bind的可读性更好，且更易用，也是Scott Meyers所推崇的方式。

> 来自[https://zhuanlan.zhihu.com/p/366654169](https://zhuanlan.zhihu.com/p/366654169)

## 事后

本来要写实验报告的我因为linux编译失败而去寻找改进方法，然后我依次找到了后面几种方法，感觉这是一个水实验报告的好机会

当我写相关内容从晚上十点写到十二点多准备上床的时候却突然灵光一闪，是不是取个地址就完美解决了，结果确实就是这样。水报告计划宣告破产，感觉自己突然像个小丑一样可怜

其实学到这么多新东西还是挺开心的（真的
