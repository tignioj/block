---
title: 创建一个空的顺序表.md
date: 2018-06-24 04:56:23
tags: [数据结构, C语言, 顺序表, typedef, 结构体]
category: 数据结构
---
# typedef
###  typedef 用法
- 用法参考 https://www.cnblogs.com/qyaizs/articles/2039101.html
```c
typedef struct tagMyStruct { 
　　　int iNum;
　　　long lLength;
} MyStruct;
```
- struct tagMyStruct 变量名
- MyStruct 变量名
```c
typedef struct {
    char number[5];
    char name[20];
    int age;
} patient;
patient p1, p2;
```
等价于
```c
struct patient {
    char number[5];
    char name[20];
    int age;
};
struct patient p1, p2;
```
好处是可以偷懒，少打几个struct,用法是一样的
### 嵌套
```c
typedef struct {
    char number[5];
    char name[20];
    int age;
} patient;
typedef patient ElemType;
```
注意，我们又重新把patient 别名为 ElemType，所以使用ElemType和使用patient是一样的
比如
`patient p1`等价于`ElemType p1`  
`patient s[20]` 等价于`ElemType s[20]`

再有
```c
typedef struct {
    int size;
    ElemType list[100];
} SequenceList;
SequenceList *p; /定义一个结构指针变量p，该指针尚为赋值，没有开辟内存，还不能通过它访问成员
```
等价于
```c
struct SequenceList { //单独定义结构，结构名为SequenceList
    int size;
    ElemType list[100];
};
struct SequenceList *p//定义一个结构指针变量p，该指针尚为赋值，没有开辟内存，还不能通过它访问成员
```
```c
SequenceList L //定义一个结构变量名为L的结构体
p = &L 
```
p指向结构L的首地址，此时我们可以既通过L访问结构成员，也可以通过指针变量p
```c
L.size = 100;
//等价于
(*p).size = 100;
//等价于
p->size = 100;
```
继续深究，发现结构体L包含成员ElemType list[100]  
我们知道一个结构体的成员时这样的  **类型名 结构成员名**
也就是说，这是个长度为100的数组，那么这是个什么类型的数组呢？  
追本溯源，找到之前对ElemType的定义
```c
typedef patient ElemType;
```
说明`ElemType list[100]`等价于`patient list[100]`,再找到patient
```c
typedef struct {
    char number[5];
    char name[20];
    int age;
} patient;
```
我们就可以知道，数组list[100]，是个结构数组，每个成员都是一个结构体
