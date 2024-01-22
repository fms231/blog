---
title: C++
date: 2023-1-31 13:33:33
description: C++整理
tags: [C++]
categories: C++
---
# C++
## 概念
变量：一块具有类型的内存（类型：数据的存储表示方式以及你可以对它进行的操作）
指针：一块内存的地址，指针的类型可能说明这个指针指向特定类型的变量（void*）
引用：可以理解为指针的一种“语法糖”（左值引用/右值引用）
数组：内存中连续排列的多个同类型变量。数组名称可以用作指向第一个元素的指针
自定义的类型（class/struct）：一组成员变量在内存里的排列方式以及可以对它进行的操作。
一个对象：按照特定排列方式存储在内存里的一组成员变量
new:分配内存，然后调用对应的构造函数（递归调用各个成员变量的构造函数）
delete:调用对应的析构函数，然后释放内存（递归调用各个成员变量的析构函数）
### 动态内存管理的两种风格（不代表只有这两种风格）
1. **RAII**（Resource Acquisition Is Initialization）：资源获取就是初始化（C++语言中可通过恰当实现构造/析构函数、恰当调用new/delete实现）
2. **垃圾回收**（C++语言中可通过智能指针实现）
### 深/浅拷贝与移动
1. 浅拷贝：拷贝指针，指向同一块内存
2. 深拷贝：拷贝内容，指向不同的内存
3. 移动：将右值的内容转移到左值上，右值的内容不再可用
函数指针：指向函数的指针
函数对象：重载了函数调用运算符()的对象
lambda表达式：匿名函数对象

## 多态
### 通过继承、虚函数实现运行时多态
#### 继承
继承的目的：代码复用
两类典型的继承：实现继承（is-a）/接口继承（has-a）
##### 实现继承
实现继承：基类的成员变量和成员函数被继承到派生类中
##### 接口继承
接口继承：基类的成员函数被继承到派生类中

#### 虚函数
虚函数的目的：实现运行时多态
虚函数：在基类中声明为虚函数的成员函数，在派生类中可以被重写

运行时多态：在运行时根据对象的实际类型来调用相应的成员函数
### 通过模板实现编译时多态
模板：对于两个只有参数类型不同的函数，不用重复编写，编译器自动生成程序中用到的不同类型的函数。模板较多的代码往往编译起来非常慢，因此模板一般放在头文件中，以便于编译器在编译时能够直接展开模板，而不是在链接时再去实例化模板。

## STL
STL主要包含容器和算法两部分
容器（container）：用来保存一系列对象的对象，例如vector、list、map等
### 迭代器
迭代器（iterator）：对指针的抽象，用来遍历容器中的对象，例如vector<int>::iterator
1. 所有迭代器都支持解引用（*）、自增（++）、自减（--）等操作
2. input iterator：只读迭代器，只能用于读取容器中的对象，单次读取，例如vector<int>::const_iterator
3. output iterator：只写迭代器，只能用于写入容器中的对象，单次写入，例如vector<int>::iterator
4. forward iterator：只读迭代器，只能用于读取容器中的对象，但是可以多次读取，例如list<int>::const_iterator
5. bidirectional iterator：双向迭代器，可以读取容器中的对象，也可以写入容器中的对象，可以多次读取，在forward iterator基础上支持自减运算符(--)，例如list<int>::iterator
### 库函数
```cpp
#include <iterator>
std::advance(it, n)：将迭代器it向前移动n个位置
std::distance(it1, it2)：计算迭代器it1和it2之间的距离
std::next(it, n)：返回迭代器it向前移动n个位置后的迭代器
std::prev(it, n)：返回迭代器it向后移动n个位置后的迭代器
```
容器的begin()和end()方法可以获得首尾迭代器
```cpp
for(auto it=c.begin(); it!=c.end(); ++it)
for(auto &x:c)
```
cbegin()和cend()方法可以获得首尾const迭代器
```cpp
for(auto it=c.cbegin(); it!=c.cend(); ++it)
for(const auto &x:c)
```
rbegin()和rend()方法可以获得反向迭代器
rcbegin()和rcend()方法可以获得反向const迭代器

### 接口
标准容器共享接口，从而发挥模板多态的特性
#### 常见的构造函数
ContainerType c(num)
ContainerType c(num, val)
ContainerType c(begin, end)
#### 容器相关的方法
```cpp
int size = c.size();
bool empty = c.empty();
c.resize(num)
c.resize(num, val)
c.clear()
```
### 常见容器
#### 栈（stack）
先进后出，只能在栈顶进行插入和删除操作
#include <stack>
C++: std::stack
push(x) 向栈顶插入元素x  
pop()   删除栈顶元素
top()   返回栈顶元素
empty() 栈是否为空
size()  栈中元素个数

n个元素入栈，出栈顺序有多少种？
出栈顺序为卡特兰数：Cn = $\frac{C^n_{2n}}{n+1}$ (n为非负整数)

#### 队列（queue）
先进先出，只能在队尾进行插入操作，只能在队首进行删除操作
#include <queue>
C++: std::queue
push(x) 向队尾插入元素x
pop()   删除队首元素
front() 返回队首元素
back()  返回队尾元素
empty() 队列是否为空
size()  队列中元素个数

queue还提供了运算符，较为常用的是使用=为两个queue赋值，使用==判断两个queue是否相等

#### 循环队列（circular queue）
循环队列是一种环形的队列，可以用数组实现。循环队列的队首和队尾是相邻的，当队尾到达数组的末尾时，如果队首不在数组的开头，可以将队尾指向数组的开头，从而实现循环队列

#### 双端队列（deque）
双端队列是一种可以在队首和队尾进行插入和删除操作的队列
C++: std::deque
#include <deque>
push_back(x)    向队尾插入元素x
push_front(x)   向队首插入元素x
pop_back()      删除队尾元素
pop_front()     删除队首元素
front()         返回队首元素
back()          返回队尾元素
empty()         双端队列是否为空
size()          双端队列中元素个数
erase(pos)      删除pos位置的元素
insert(pos, x)  在pos位置插入元素x

#### 优先队列（priority queue）
优先队列是一种可以在队尾插入元素，队首删除元素，但是删除的元素是队列中优先级最高的元素的队列
#include <queue>
C++: std::priority_queue
push(x)     向队尾插入元素x
pop()       删除队首元素
top()       返回队首元素
empty()     优先队列是否为空
size()      优先队列中元素个数

#### 向量（vector）
向量是一种可以在任意位置插入和删除元素的容器，但是插入和删除元素的效率较低
#include <vector>
C++: std::vector
push_back(x)    向向量尾部插入元素x
pop_back()      删除向量尾部元素
front()         返回向量首部元素
back()          返回向量尾部元素
empty()         向量是否为空
size()          向量中元素个数
erase(pos)      删除pos位置的元素
insert(pos, x)  在pos位置插入元素x

#### 列表（list）
列表是一种可以在任意位置插入和删除元素的容器，但是插入和删除元素的效率较高
#include <list>
C++: std::list
push_back(x)    向列表尾部插入元素x
push_front(x)   向列表首部插入元素x
pop_back()      删除列表尾部元素
pop_front()     删除列表首部元素
front()         返回列表首部元素
back()          返回列表尾部元素
empty()         列表是否为空
size()          列表中元素个数
erase(pos)      删除pos位置的元素
insert(pos, x)  在pos位置插入元素x

#### 集合（set）
集合是一种不包含重复元素的容器，集合中的元素是有序的
#include <set>
C++: std::set
insert(x)   向集合中插入元素x
erase(x)    删除集合中元素x
find(x)     查找集合中元素x
empty()     集合是否为空
size()      集合中元素个数

#### 映射（map）
映射是一种键值对的容器，键是唯一的，值可以重复
#include <map>
C++: std::map
insert(pair)    向映射中插入键值对
erase(x)        删除映射中键为x的键值对
find(x)         查找映射中键为x的键值对
empty()         映射是否为空
size()          映射中键值对个数


