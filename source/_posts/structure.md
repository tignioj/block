---
title: 结构体
date: 2018-06-24 05:08:34
tags: [结构体, C语言]
category: C语言
---
## 结构体
过去，我们一般定义一个结构体是这样的

- 单独定义
```c
struct 结构名 {
    类型名 结构成员名1；
    类型名 结构成员名2；
    类型名 结构成员名3；
    ...
};
struct 结构名 结构变量名1， 结构变量名2...
//例子
struct student {
    int num;
    char name[20];
    int score;
};
struct student stu1, stu2;
//操作
stu1.name = "zhangsan";
stu1.num = 001;
```


- 混合定义
```c
struct 结构名 {
    类型名 结构成员名1；
    类型名 结构成员名2；
    类型名 结构成员名3；
    ...
} 结构变量名表;
//例子
struct student {
    int num;
    char name[20];
    int score;
} stu1, stu2;
//操作
stu1.name = "xiaoming";
stu1.num = 002;
```
- 无类型名定义
  
无类型名定义是指在定义结构变量是省略结构名  
**注意** 由于没有给出结构名，在此定义语句后面无法再定义这个类型的其他结构变量，除非把定义过程在写一遍，一般情况下，除非变量不会再增加，还是建议采用前两种结构变量的定义方式。
```c
struct {
    类型名 结构成员名1；
    类型名 结构成员名2；
    类型名 结构成员名3；
    ...
} 结构变量名表;
//例子
struct student {
    int num;
    char name[20];
    int score;
}stu1, stu2;
//操作
stu1.name = "zhangsan";
stu1.num = 001;
```

---



## 结构体操作
- 用一个点`.`
```c
s1.num = 1;
s1.name = "zhang";
```
- 用`->`
```c
s1->num = 2;
s1->name = "zhang";
```

## 结构指针

```c
struct student s1 = {101, "zhangsan", 95}, *p;
p = &s1;
```
结构指针的值实际上是结构变量的**首地址**，即第一个成员的首地址，有了结构指针的定义，既可以通过结构变量s1直接访问结构成员，也可以通过结构指针变量p间接访问它所指向的结构变量中的各个成员，具体有两种形式
- 用 **\*p**
```c
(*p).num = 101;
```
**括号必不可少，因为点运算符优先级高于“\*”运算符**
- 用指向运算符`->`访问指针指向的结构成员
```c
p->num = 101;
```
使用结构指针访问结构成员时，通常使用指向运算符，以下三条语句等价
```c
s1.num = 101;
(*p).num = 101;
p->num = 101;
```
# 嵌套结构体
- 引例
```c
struct address { //定义地址结构
    char city[10];
    char street[20];
    int code;
    int zip;
};
struct student {
    int num;
    char name[20];
    int score;
    struct address addr; //定义通信地址成员
};
```
- 操作
```c
struct student st1;
s1.addr.zip = 310015;
```
- 结构变量可以整体赋值，例如
```c
s1 = s2;
//等效于下面语句
s2.num = s1.num;
strcpy(s2.name, s1.name);
s2.addr = s1.addr;
s2.score = s1.score;
```
## 注意
**1.只有相同结构类型的变量之间才可以直接赋值**  
**2.定义嵌套结构时，必须先定义成员的结构类型，在定义主结构类型**，比如前面的先定义结构address，再定义student
