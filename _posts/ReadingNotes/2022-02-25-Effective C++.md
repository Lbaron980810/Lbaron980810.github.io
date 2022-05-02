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







# Bottom













