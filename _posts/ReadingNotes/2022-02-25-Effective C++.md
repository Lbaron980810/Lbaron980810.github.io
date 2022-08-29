---
title: Effective C++阅读笔记
tags: C++
category: ReadingNotes
---

# Effective C++ 阅读笔记

## 0 导读

1. explicit的作用 P5

```cpp
class A {
    public:
    explicit A(int x); //不是default构造函数
}
void dosomething(A obj);
dosomething(A(20)); //可行的，相当于用A的构造函数进行了显式转换
```

将函数声明成explicit，可以阻止他们被用来执行**隐式类型转换**，但他们仍可被用来进行**显式类型转换**





# Essential C++ 阅读笔记

## 5.4 定义一个抽象基类

1. 找出所有子类共通的操作行为
2. 设法找出哪些操作行为与类型相关，也就是哪些操作行为必须根据不同的派生类而又不同的实现方式。这些操作行为应该成为整个类的继承体系中的**虚函数**
3. 找出每个操作的访问层级







# Bottom













