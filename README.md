# Cpp 2024 春季招聘面试笔记

This is the repository I use to keep track of spring 2024 recruiting (c++)

这是我用于记录 2024 春招(c++)的仓库

推荐使用Typora阅读

[toc]



- [Cpp 2024 春季招聘面试笔记](#cpp-2024-春季招聘面试笔记)
    - [计算机基础](#计算机基础)
      - [原码 反码 补码](#原码-反码-补码)
      - [大端 vs 小端](#大端-vs-小端)
    - [C 语言](#c-语言)
      - [函数参数入栈顺序及 i++,++i 实现](#函数参数入栈顺序及-ii-实现)
      - [指针运算](#指针运算)
      - [函数指针](#函数指针)
      - [指针函数](#指针函数)
      - [函数指针数组](#函数指针数组)
      - [隐式类型转换法则](#隐式类型转换法则)
      - [位操作](#位操作)
      - [空指针 \&\& 野指针](#空指针--野指针)
      - [回调函数](#回调函数)
      - [生成随机数](#生成随机数)
      - [逗号表达式运算法则](#逗号表达式运算法则)
    - [C++](#c)
      - [C++和C的不同以及面向对象的了解](#c和c的不同以及面向对象的了解)
      - [static关键字](#static关键字)
      - [new和malloc区别](#new和malloc区别)
      - [多态的实现](#多态的实现)
      - [STL](#stl)
      - [自定义函数比较器](#自定义函数比较器)
    - [算法](#算法)
      - [排序算法](#排序算法)
      - [单调队列](#单调队列)
      - [单调栈](#单调栈)
    - [Linux基础](#linux基础)
      - [静态库和动态库](#静态库和动态库)
    - [数据库](#数据库)
    - [系统编程](#系统编程)
    - [网络编程](#网络编程)
      - [I/O复用](#io复用)

### 计算机基础

---

#### 原码 反码 补码

##### 原码

原码，符号位加真值的绝对值

```
+3[原] = 00000011
-3[原] = 10000011
```

##### 反码

正数的反码是其原码

负数的反码，符号位不变，其余各位`~`(取反)

```
+3[反] = 00000011[原] = 00000011[反]
-3[反] = 10000011[原] = 11111100[反]
```

##### 补码

正数的补码是其原码

负数的补码，是其反码+1

```
+3[补] = 00000011[原] = 00000011[反] = 00000011[补]
-3[补] = 00000011[原] = 11111100[反] = 11111101[补]
```

反码和补码，不能直接看出其实际的数值， 需要转换成原码后计算

数据在内存中均以补码形式存储，方便计算(做加法无需考虑符号)

#### 大端 vs 小端

##### 什么是字节序

字节顺序，又称端序或尾序（英语：Endianness），在计算机科学领域中，指电脑内存中或在数字通信链路中，组成多字节的字的字节的排列顺序。

在几乎所有的机器上，多字节对象都被存储为连续的字节序列。例如在 C 语言中，一个类型为 int 的变量 x 地址为 0x100，那么其对应地址表达式&x 的值为 0x100。且 x 的四个字节将被存储在电脑内存的 0x100, 0x101, 0x102, 0x103 位置。

字节的排列方式有两个通用规则。例如，将一个多位数的低位放在较小的地址处，高位放在较大的地址处，则称小端序；反之则称大端序。在网络应用中，字节序是一个必须被考虑的因素，因为不同机器类型可能采用不同标准的字节序，所以均按照网络标准转化。

例如假设上述变量 x 类型为 int，位于地址 0x100 处，它的值为 0x01234567，地址范围为 0x100~0x103 字节，其内部排列顺序依赖于机器的类型。大端法从首位开始将是：0x100: 0x01, 0x101: 0x23,..。而小端法将是：0x100: 0x67, 0x101: 0x45,..。

##### 大端序

![大端序](imgs/image.png)

低位高地址

##### 小端序

![小端序](imgs/image-1.png)

低位低地址

> tips: 现代PC大多采用小端字节序，因此小端字节序又被称为主机字节序。大端字节序也称为网络字节序，它给所有接收数据的主机提供了一个正确解释收到的格式化数据的保证。

**参考**

[维基百科-字节序](https://zh.wikipedia.org/zh-cn/%E5%AD%97%E8%8A%82%E5%BA%8F)

### C 语言

---

#### 函数参数入栈顺序及 i++,++i 实现

i++

```c
// i++
int j = i;
i = i + 1;
return j;
```

++i

```c
// ++i
i = i + 1;
return i;
```

```c
int i = 0;
printf("%d %d %d %d\n", ++i, i++, i++, ++i); // 4 2 1 4
```

#### 指针运算
指针不能与指针相加，但可以相减和比较。如：

```cpp
int b[N];
int l = lower_bound(b + 1, b + n + 1, b[mid]) - b;
int r = upper_bound(b + 1, b + n + 1, b[mid]) - b - 1;
//使用 l r 记录

```

#### 函数指针

本质上是一个指针，指向一个函数

- 定义函数指针

`void (*fp)(int a, int b);`

函数指针是专用的，格式要求很强，返回值、参数类型、参数个数都必须相同

- 如何给函数指针赋值

```c
void f(int a, int b) {

}

void (*fp)(int a, int b) = data;
```

- 如何调用函数指针

```c
p2(a, b);
(*p2)(a, b);
```

#### 指针函数

本质是一个函数，返回值是一个指针

```c
int ret;

int *add(int a, int b) {
    ret = a + b;
    return &ret;
}

int main(int argc, char*argv[]) {

    int a = 10;
    int b = 20;

    int *p = add(a, b);

    printf("%d\n", *p);

    return 0;
}

```

#### 函数指针数组

声明一个数组，里面存储的类型是，指向函数的指针。

```c
int Add(int x, int y) {
    return x + y;
}

int Sub(int x, int y) {
    return x - y;
}

int MUl(int x, int y) {
    return x * y;
}

int Div(int x, int y) {
    return x / y;
}

int main(int argc, char*argv[]) {

    int (*pf1)(int, int) = Add;
    int (*pf2)(int, int) = Sub;
    int (*pf3)(int, int) = Mul;
    int (*pf4)(int, int) = Div;

    // 函数指针数组
    int (*pf[4])(int, int) = {Add, Sub, Mul, Div};

    // 调用

    for (int i = 0;i < 4; i++) {
        pf[i](2, 5);
    }
}

```

#### 隐式类型转换法则

若参与运算的数据类型相同则运算所得结果的数据类型也为该数据类型。若参与运算的数据类型不同，则先转换成同一类型，然后进行运算。

1. 转化按数据长度增加的方向进行，以保证精度不降低。例如 int 类型和 long 类型运算时，先把 int 类型转换成 long 类型后再进行运算
2. 即当参加算数或比较运算的两个操作数类型不统一时，将简单类型向复杂类型转换

$$char->short->int->long->float->double$$

3. 在赋值语句中，赋值号两边数据类型一定是相兼容的类型，如果等号两边数据类型不兼容，语句在编译时会报错

#### 位操作

设置第 N 位：`Number |= (1ul << nth Position)`

设置第 N 位意味着如果第 N 位为 0，则将其设置为 1，如果为 1，则保持不变。在 C 中，按为或运算用于设置整数数据类型的位。据我们所知 | 计算一个新的整数值，其中每个位的位置只有当操作数(数据类型)在该位置为 1 时才为 1。

简而言之，如果其中任何一位为 1，则可以说两位的按位与始终为 1

0 | 0 = 0  
1 | 0 = 1  
0 | 1 = 1  
1 | 1 = 1

清除位：`Number &= ~(1UL << nth Position)`

清除位意味着如果第 N 位为 1，则将其清为 0，如果为 0，则保持不变。按位与运算符用于清除位整数数据类型。如果其中任何一位为零，则两位的“与”始终为零。

0 & 0 = 0  
1 & 0 = 0  
0 & 1 = 1

检查位:`Bit = Number & (1UL << nth Position)`

要检查第 n 位，先将第 n 个“1” 位置向左移动，然后将其与数字“与”

切换位：`Numebr ^ = (1UL << nth Position)`

切换位表示如果第 N 位为 1，则将其更改为 0， 如果为 0， 则将其更改为 1。按位异或运算符用于切换整数数据类型的位。要切换第 n 个位移位，将第 n 个位置的'1'向左移动并异或它

0 ^ 0 = 1  
1 ^ 0 = 1  
0 ^ 1 = 0  
1 ^ 1 = 1

#### 空指针 && 野指针

##### 空指针

指向地址 0 的指针

C 语言中

```c
#define NULL ((void *)0) // msvc
int *p = NULL;
```

C++

```cpp
#define NULL 0 // msvc
```

C++11 中加入`nullptr`

NULL 与 nullptr 比较

```cpp
void func(int n);
void func(char *s);

func( NULL );
```

使用如上的函数重载时，在调用`func( NULL )`时，我们期望`void func(char *s);`被调用，而实际上`NULL`被解释为 0，因此编译器将调用`void func(int n);`

在 C++11 中，nullptr 是一个新关键字，可以（并且应该！）用于表示 NULL 指针；

**Notice:**

> 空指针不能被解引用，类似`int *p = NULL; *p = 2;`的语句会引起编译错误

##### 野指针

定义：

> 一个指针既不引用合法对象，也不为 NULL

产生野指针(wild pointer)的原因

- 未初始化的指针对象
- 已不存在的对象
- 计算的指针值
- 不正确对齐的指针值
- 指针本身或其指向内容的意外损坏
- ...

```c
int main(void)
{

   int *p;  // uninitialized and non-static;  value undefined 未初始化且非static 值未定义
   {
      int i1;
      p = &i1;  // valid 有效的
   }            // i1 no longer exists;  p now invalid    i1不再存在 p现在无效

   p = (int*)0xABCDEF01;  // very likely not the address of a real object 很可能不是真实存在的对象地址

   {
      int i2;
      p = (int*)(((char*)&i2) + 1);  // p very likely to not be aligned for int access p很可能未针对 int 访问进行对齐
   }

   {
      char *oops = (char*)&p;
      oops[0] = 'f';  oops[1] = 35;  // p was clobbered p被破坏
   }
}
```

**参考**

[stack overflow \`What is the meaning of "wild pointer" in C?\` 高分回答](https://stackoverflow.com/a/2584552)

#### 回调函数

回调函数就是利用函数指针来，在函数内部调用不同的函数

```c
void test(){
    printf("hehe!\n");
}
void print_hehe(void (*pfun)(void)){
    if (1){
        pfun();
    }
}
int main(){
    print_hehe(&test);
}
```

#### 生成随机数

```c
rand() // 函数 随机发生器
int rand() // 用法
csdlib // 所在头文件
```

rand() 会根据当前的随机数种子，根据线性同余法生成一段数列，因这段数列周期特别长故在一定的范围里可以看成是随机的
rand() 返回一随机数值的范围在0~RAND_MAX之间。RAND_MAX的范围最少是在32767之间。0~RAND_MAX每个数字被选中的几率是相同的。
用户未设定随机数种子时，系统默认的随机数种子为 1 
rand()产生的是伪随机数字，每次执行时是相同的，若要不同，用函数srand()初始化它。

```c
srand() // 函数， 初始化随机数发生器
void srand(unsigned int seed) // 用法
stdlib.h // 所在头文件
```

srand() 用来设置rand()产生随机数时的随机数种子。参数seed必须是个整数，如果每次seed都设置相同值，每次都会一样

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    srand(1);
    int i;

    for (i = 0;i < 5; ++i) {
        printf("%d", rand());
    }
    return 0;
}
```

#### 逗号表达式运算法则

1. 逗号表达式的运算过程为：从左往右逐个计算表达式
2. 逗号表达式作为一个整体，它的值为最后一个表达式的值
3. 逗号运算符的优先级别在所有运算符中最低

```c
(a = 3, b = 5, b += a, c = b * 5)
// 逗号表达式的值 40
```

### C++

---

#### C++和C的不同以及面向对象的了解

1) C语言是面向过程的程序设计，主要核心为：数据结构和算法，具有高效的特性。对于C语言程序的设计，主要是考虑如何通过一个过程，对输入进行处理得出一个输出。C++是面向对象的程序设计，对于C++，首先考虑的是如何构造一个对象模型，让这个模型配合对应问题，这样可以通过获取对象状态信息得到输出
2) C++对比C增强点
   1) 命名空间
   2) 变量检测增强
   3) struct增强
   4) 面向对象
   5) 面向对象三大特性：封装、继承、多态

#### static关键字

1. 定义全局静态变量和局部静态变量
2. 定义静态函数：在函数返回类型前加上static关键字，函数即被定义为静态函数，静态函数只能在本源文件中使用；
3. 在变量类型前加上static关键字，变量即被定义为静态变量。静态变量只能在本源文件中使用
4. c++中，static关键字可以用于定义类中的静态成员变量，所有对象的静态数据成员都共享这一块静态存储空间
5. 在c++中，static关键字可以用于定义类中的静态成员函数

#### new和malloc区别

1. new是操作符，而malloc是函数
2. new在调用的时候先分配内存，再调用构造函数，释放的时候调用析构函数。而malloc没有构造函数和析构函数
3. malloc需要给定申请内存的大小，返回的指针需要强转
4. new可以被重载，malloc不行
5. new分配内存更直接和安全
6. new发生错误抛出异常，malloc返回null

#### 多态的实现

##### 类的虚函数表

- 每个包含了虚函数的类都包含一个虚表
- 当一个类(B)继承另一个类(A)时，类B会继承类A的函数的调用权。所以如果一个基类包含了虚函数，那么其继承类也可调用这些虚函数，换句话说，一个类继承了包含虚函数的基类，那么这个类也拥有自己的虚表

```cpp
class A{
public:
    virtual void vfunc1();
    virtual void vfunc2();

    void func1();
    void func2();

private:
    int m_data1, m_data2;
};

class B: public A{ // 此时类B也拥有自己的虚表

};
```

![alt text](imgs/image-6.png)

- 虚表是一个存放指针的数组，其内的元素是虚函数的指针，每个元素对应一个虚函数的函数指针。需要指出的是：普通的函数即非虚函数，其调用并不需要经过虚表，所以虚表的元素并不包括普通函数的函数指针
- 虚表内的条目，即虚函数指针的赋值发生在编译器的编译阶段，也就是说在代码的编译阶段，虚表就可以构造出来了

##### 虚表指针

- 虚表是属于类的，而不是某个具体的对象，一个类只需要一个虚表，同一个类的所有对象都使用同一个虚表
- 为了指定对象的虚表，对象内部包含一个虚表的指针，来指向自己所使用的虚表。为了让每个包含虚表的类的内部都有一个虚表指针，编译器在类中添加了一个指针，*_vptr, 用来指向虚表。这样当类的对象在创建时便拥有了这个指针，且这个指针的值会自动被设置为指向类的虚表

```cpp
class A{
public: 
    virtual void vfunc1();
    virtual void vfunc2();
    void func1();
    void func2();
private: 
    int m_data1, m_data2;
};

class B : public A{
public:
    virtual void vfunc1();
    void func2();

private:
    int m_data3;
};

class C : public B {
public : 
    virtual void vfunc2();
    void func2();
private: 
    int m_data1, m_data4;
};
```

![alt text](imgs/image-7.png)

#### STL

- container
  - [vector](#vector)
  - [set](#set)
  - [unordered_set](#unordered_set)
  - [unordered_map](#unordered_map)
- Algorithms
- iterators


C++ STL（标准模板库）是一套功能强大的 C++ 模板类，提供了通用的模板类和函数，这些模板类和函数可以实现多种流行和常用的算法和数据结构，如向量、链表、队列、栈。

C++ 标准模板库的核心包括以下三个组件：

| 组件               | 描述                                                                                             |
| ------------------ | ------------------------------------------------------------------------------------------------ |
| 容器（Containers） | 容器是用来管理某一类对象的集合。C++ 提供了各种不同类型的容器，比如 deque、list、vector、map 等。 |
| 算法（Algorithms） | 算法作用于容器。它们提供了执行各种操作的方式，包括对容器内容执行初始化、排序、搜索和转换等操作。 |
| 迭代器（iterators） | 迭代器用于遍历对象集合的元素。这些集合可能是容器，也可能是容器的子集。 |

[菜鸟教程](https://www.runoob.com/cplusplus/cpp-stl-tutorial.html)

##### vector

`vector`与动态数组相同，能够在插入或删除元素时自动调整自身大小，其存储由容器自动处理。`vector`元素被放置在连续的内存中，以便可以使用迭代器访问和遍历它们。在`vector`中，数据插入到末尾。在末尾插入需要不同的时间，因为有时可能需要扩展数组。删除最后一个元素只需要O(1)时间，因为不会发生大小调整。在开头或中间插入和擦除在时间上是线性的。

**Syntax**

```cpp
std::vector<dataType> vectorName;
```

常用函数

- `begin()`、`end()`
- `size()`、`capacity()`、`empty()`
- `at(g)`、`front()`、`back()`
- `push_back()`、`pop_back()`、`clear()`、`earse()`、`emplace()`、`emplace_back()`

##### set

`set`是一种关联容器，其中每个元素都必须是唯一的，因为元素的值可以标识它。这些值以特定的排序顺序存储，即升序或降序。

使用二叉搜索树实现

**Syntax**

```cpp
std::set <data_type> set_name;
```

常用函数

* `begin()`
* `end()`
* `size()`
* `max_size()`
* `empty()`

##### unordered_set

`unordered_set`是使用哈希表实现的无序关联容器，其键被哈希到哈希表的索引中，以便插入始终是随机的。`unordered_set`上的所有操作平均需要常数时间 O(1)，在最坏的情况下可以达到线性时间 O(n)，这取决于内部使用的哈希函数，但实际上它们执行得非常好并且通常提供常数时间查找操作。

**Syntax**

```cpp
std::unordered_set<data_type> name;
```

常用函数

- `size()`、`empty()`
- `find()`
- `insert()`
- `erase()`、`clear()`

##### unordered_map

`unordered_map`是一个关联容器，用于存储由键值和映射值组合形成的元素。键值用于唯一标识元素，映射值是与键关联的内容。键和值都可以是任何预定义或用户定义的类型。简单来说，`unordered_map`就像一个字典类型的数据结构，其本身存储元素。它包含连续的对（键、值），允许根据其唯一键快速检索单个元素。

`unordered_map` 内部使用哈希表实现，提供给映射的键被哈希为哈希表的索引，这就是为什么数据结构的性能很大程度上依赖于哈希函数，但平均而言，搜索、插入和删除的成本哈希表的复杂度为 O(1)。

`operator[]` 和 `insert()`

`operator[]`： 如果键已经存在于map中，`operator[]`不会覆盖现有值；相反，它返回对现有值的引用。

`insert()`： 如果键已经存在于map中，insert()不会执行任何插入，并返回到具有相同键的现有元素的迭代器。

#### 自定义函数比较器

- 重写`struct`的`operator<`方法
- 定义Comparator函数
- 定义Comparator结构体对象

1. 自定义的`struct`

```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
struct Str {
public:
  bool operator<(const Str &r) const { return s.length() < r.s.length(); }
  std::string s;
  Str(std::string str) : s(str){};
};

int main() {
  std::vector<Str> vec = {Str("1234"), Str("456"), Str("11")};

  sort(vec.begin(), vec.end());

  for (auto it : vec) {
    std::cout << it.s << " ";
  }

  std::cout << "\n";
  return 0;
}
```

Output

```
11 456 1234
```

2. 函数比较器

```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>

bool cmp(const std::string& s1, const std::string& s2){
    return s2.length() < s1.length();
}


int main() {
    std::vector<std::string> vec = {"12", "123", "1234"};
    
    sort(vec.begin(), vec.end(), cmp);
    
    for (auto& s: vec){
        std::cout << s << " ";
    }
    
    std::cout << "\n";
    
    return 0;
}
```

Output

```
1234 123 12
```

3. 函数对象比较器

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

struct myCmp{
    bool operator()(
    const std::vector<int>& l,
    const std::vector<int>& r
    ){
        return l[0] < r[0];
    }
};


int main() {
    std::vector<std::vector<int>> vec = {{1,2}, {7, 8}, {4, 6}};
    
    sort(vec.begin(), vec.end(), myCmp());
    
    for (auto& it: vec){
        std::cout << "{" << it[0] << " " << it[1] << "}" << " ";
    }
    
    std::cout << "\n";
    
    return 0;
}

```

Output

```
{1 2} {4 6} {7 8}
```

### 算法

---

#### 排序算法

##### 选择排序

```cpp
void selectionSort(vector<int> &nums) {
    for (int i = 0; i < nums.size() - 1; i++) {
        int min = i;
        for (int j = i; j < nums.size(); j++) {
            if (nums[j] < nums[min]) min = j;
        }
        swap(nums[min], nums[i]);
    }
}
```

时间复杂度：

$$\mathcal{O(n^2)}$$

空间复杂度：

$$\mathcal{O(1)}$$

稳定性：不稳定

##### 冒泡排序

```cpp
void bubbleSort(vector<int> &nums) {
    for (int i = 0; i < nums.size() - 1; i++) {
        bool isSwap = false;
        for (int j = 0; j < nums.size() - i - 1; j++) {
            if (nums[j] > nums[j + 1]) {
                swap(nums[j], nums[j + 1]);
                isSwap = true;
            }
        }
        if (!isSwap) break;
    }
}
```

时间复杂度：

$$\mathcal{O(n^2)}$$

空间复杂度：

$$\mathcal{O(1)}$$

稳定性：稳定

##### 插入排序

```cpp
void insertionSort(vector<int> &nums) {
    for (int i = 1; i < nums.size(); i++) {
        int base = nums[i];
        int j = i - 1;
        while (j >= 0 && nums[j] > base) {
            nums[j + 1] = nums[j];
            j--;
        }
        nums[j + 1] = base;
    }
}
```

时间复杂度：

$$\mathcal{O(n^2)}$$

空间复杂度：

$$\mathcal{O(1)}$$

稳定性：稳定

##### 快速排序

```cpp
int partition(vector<int> &nums, int left, int right) {
    int i = left;
    int j = right;

    while (i < j) {
        while (nums[i] < nums[left] && i < j) i++; // 找到第一个大于x的数
        while (nums[j] > nums[left] && i < j) j--; // 找到第一个小于x的数
        swap(nums[i], nums[j]);
    }
    swap(nums[left], nums[i]);
    return i;
}

void quickSort(vector<int> &nums,int left, int right) {
    if (left >= right) return;
    int pivot = partition(nums, left, right);
    quickSort(nums, left, pivot-1);
    quickSort(nums, pivot+1, right);
}
```

时间复杂度：

$$\mathcal{O(n \log n)}$$

空间复杂度：

$$\mathcal{O(\log n)}$$

##### 归并排序

```cpp
void merge(vector<int> &nums, int left, int mid, int right) {
    vector<int> tmp(right - left + 1, 0);
    int i = left;
    int j = mid+1;
    int k = 0;
    while (i <= mid && j <= right) {
        if (nums[i] < nums[j]) {
            tmp[k++] = nums[i++];
        } else
            tmp[k++] = nums[j++];
    }
    while (i <= mid) tmp[k++] = nums[i++];
    while (j <= mid) tmp[k++] = nums[j++];
    for (int k = 0; k < right - left + 1; k++) {
        nums[left + k] = tmp[k];
    }
}

void mergeSort(vector<int> &nums, int left, int right) {
    if (left >= right) return;
    int mid = (left + right) >> 1;
    mergeSort(nums, left, mid);
    mergeSort(nums, mid + 1, right);
    merge(nums, left, mid, right);
}
```

时间复杂度：

$$\mathcal{O(n \log n)}$$

空间复杂度：

$$\mathcal{O(n)}$$

##### 堆排序

```cpp
void siftDown(vector<int> &nums, int n, int i) { //n为堆长度， 从i开始向下堆化
    while (true) {
        int l = 2 * i + 1;
        int r = 2 * i + 2;
        int ma = i;
        if (l < n && nums[l] > nums[i]) {
            ma = l;
        }
        if (r < n && nums[r] > nums[i]) {
            ma = r;
        }
        if (ma == i) break; // 无需堆化
        swap(nums[ma], nums[i]);
        i = ma;
    }

}

void heapSort(vector<int>& nums) {
    // 从下往上堆化 除叶子节点
    for (int i = nums.size() / 2 - 1; i >= 0; i--) {
        siftDown(nums, nums.size(), i);
    }

    for (int i = nums.size()-1; i > 0; i--) {
        swap(nums[i], nums[0]);
        siftDown(nums, i, 0);
    }
}
```

时间复杂度：

$$\mathcal{O(n \log n)}$$

空间复杂度：

$$\mathcal{O(1)}$$

排序算法比较
![alt text](imgs/image-2.png)

参考  
[hello 算法](https://www.hello-algo.com/chapter_sorting/summary/#1)

#### 单调队列

[239.滑动窗口最大值 | leetcode](https://leetcode.cn/problems/sliding-window-maximum/description/)

#### 单调栈

[739.每日温度 | leetcode](https://leetcode.cn/problems/daily-temperatures/description/)

### Linux基础

#### 静态库和动态库

##### 静态库

链接阶段，会将汇编生成的目标文件.o与引用到的库一起链接打包到执行文件中。

特点：

- 静态库对函数库的链接是放在编译时期完成的
- 程序在运行时与函数库再无瓜葛，移植方便
- 浪费空间和资源，因为所有相关的目标文件与牵涉到的函数库被链接合成一个可执行文件。

Linux创建动态库步骤

1. 编写库头文件

```cpp
#ifndef __staticmath__h__
#define __staticmath__h__

class StaticMath {
public:
  StaticMath(void);
  ~StaticMath(void);

  static double add(double a, double b);
  static double sub(double a, double b);
  static double mul(double a, double b);
  static double div(double a, double b);

  void print();
};

#endif //__staticmath__h__

```

2. 源文件

```cpp
#include "StaticMath.h"
#include <iostream>

StaticMath::StaticMath(void) {}

StaticMath::~StaticMath(void) {}

double StaticMath::add(double a, double b) { return a + b; }

double StaticMath::sub(double a, double b) { return a - b; }

double StaticMath::mul(double a, double b) { return a * b; }

double StaticMath::div(double a, double b) { return a / b; }

void StaticMath::print() { std::cout << "Static Math Library" << std::endl; }

```

3. 将代码文件编译成目标文件.o

```bash
g++ -c StaticMath.cpp
```

4. 通过ar工具将目标文件打包成.a静态库文件

```bash
ar -crv libstaticmath.a StaticMath.o
```

![alt text](imgs/image-3.png)

使用静态库

编写测试代码

```cpp
#include "StaticMath.h"
#include <cstdlib>
#include <iostream>

using namespace std;

int main() {
  double a = 10;
  double b = 2;

  cout << "a + b = " << StaticMath::add(a, b) << endl;
  cout << "a - b = " << StaticMath::sub(a, b) << endl;
  cout << "a * b = " << StaticMath::mul(a, b) << endl;
  cout << "a / b = " << StaticMath::div(a, b) << endl;

  StaticMath sm;
  sm.print();

  return 0;
}

```

编译时指定静态库搜索路径(-L)、指定静态库名(不包含lib前缀和.a后缀, -l)

```bash
g++ TestStaticLibrary.cpp -L./ -lstaticmath
```

![alt text](imgs/image-4.png)

##### 动态库

动态库在内存中只存在一份拷贝，避免了静态库浪费空间的问题

特点：
- 动态库把一些对库函数的链接载入推迟到程序运行的时期
- 可以实现进程之间的资源共享
- 将一些程序升级变得简单
- 甚至可以真正做到链接载入完全由程序员在程序代码中控制（显示调用）

Linux下创建与使用动态库

动态链接库的名字形式为libxxx.so，前缀为lib, 后缀名为'.so'

- 针对实际库文件，每个动态库都有个特殊的名字'soame'。在程序启动后，程序通过这个名字来告诉动态加载器该载入哪个共享库
- 在文件系统中，soname仅是一个链接到实际动态库的链接。对于动态库而言，每个库实际上都有另一个名字给编译器用。它是一个指向实际库镜像文件的链接文件(lib+soname+.so)

创建动态库

编写库代码

```cpp 头文件
#ifndef __dynamicmath__h__
#define __dynamicmath__h__

class DynamicMath {
public:
  DynamicMath(void);
  ~DynamicMath(void);

  static double add(double a, double b);
  static double sub(double a, double b);
  static double mul(double a, double b);
  static double div(double a, double b);

  void print();
};
#endif // !__dynamicmath__h__

```

```cpp
#include "DynamicMath.h"
#include <iostream>

DynamicMath::DynamicMath(void) {}

DynamicMath::~DynamicMath(void) {}

double DynamicMath::add(double a, double b) { return a + b; }

double DynamicMath::sub(double a, double b) { return a - b; }

double DynamicMath::mul(double a, double b) { return a * b; }

double DynamicMath::div(double a, double b) { return a / b; }

void DynamicMath::print() { std::cout << "so" << std::endl; }

```

首先生成目标文件 加编译器选项-fpic

```bash
g++ -fPIC -c DynamicMath.cpp
```
-fPIC创建与地址无关的编译程序

然后生成动态库， 此时要加编译器选项 -shared

```bash
g++ -shared -o libdynmath.so DynamicMath.o
```

引用动态库编译可执行文件

```bash
g++ TestDynamicLibrary.cpp -L./ -ldynmath
```

创建环境变量

```bash
g++ TestDynamicLibrary.cpp -L./ -ldynmath
```

![alt text](imgs/image-5.png)

### 数据库

---

### 系统编程

---

### 网络编程

---

#### I/O复用

I/O复用使程序能同时监听多个文件描述符。

I/O复用虽然能同时监听多个文件描述符，但它本身是阻塞的。 并且当多个文件描述符同时就绪时，如果不采取额外的措施，程序就只能按顺序依次处理其中的每一个文件描述符，这使得服务器程序看起来像是串行工作的。如果要实现并发，只能使用多进程或多线程等手段

Linux下实现I/O复用的系统调用主要有select、poll和epoll

##### select系统调用

原型

```c
#include <sys/select.h>
int select(int nfds, fd_set* readfds, fd_set* writefds, fd_set* exceptfds, struct timeval* timeout);
```

- nfds 指定被监听的文件描述符的总数。通常设置为select监听的所有文件描述符中的最大值加1
- readfds、writefds和exceptfds参数分别指向可读、可写和异常等事件对应的文件描述符集合


##### poll系统调用

poll系统调用和select类似，也是在指定时间内轮询一定数量的文件描述符，以测试其中是否有就绪这

```c
#include <poll.h>
int poll(struct pollfd* fds, nfds_t nfds, int timeout);
```

##### epoll系列系统调用

内核事件表

epoll是Linux特有的I/O复用函数。他在实现和使用上与select、poll有很大差异。首先,epoll使用一组函数来完成任务,而不是单个函数。其次,epoll把用户关心的文件描述符上的事件放在内核 里的一个事件表中,从而无须像select和poll那样每次调用都要重复传入文件描述符集或事件集。但epoll需要使用一个频外的文件描述符,来唯一标识内核中的这个事件表。这个文件描述符使用如下epoll_create函数来创建：

```c
#include <sys/epoll.h>
int epoll_creat(int size)
```

size参数现在并不起作用，只是给内核一个提示，告诉它事件表需要多大，该函数返回的文件描述符将用作其他epoll系统调用的第一个参数，以指定要访问的内核事件表


