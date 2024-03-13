# Cpp 2024春季招聘面试笔记

This is the repository I use to keep track of spring 2024 recruiting (c++)

这是我用于记录2024春招(c++)的仓库


- [Cpp 2024春季招聘面试笔记](#cpp-2024春季招聘面试笔记)
    - [计算机基础](#计算机基础)
      - [原码 反码 补码](#原码-反码-补码)
      - [大端vs小端](#大端vs小端)
    - [C语言](#c语言)
      - [函数参数入栈顺序及i++,++i实现](#函数参数入栈顺序及ii实现)
      - [函数指针 \&\& 函数指针数组 \&\& 指向函数指针数组的指针](#函数指针--函数指针数组--指向函数指针数组的指针)
      - [隐式类型转换法则](#隐式类型转换法则)
      - [设置、清除、切换和检查单个位](#设置清除切换和检查单个位)
      - [空指针 \&\& 野指针](#空指针--野指针)
    - [C++](#c)
      - [STL](#stl)
    - [算法](#算法)
      - [排序算法](#排序算法)
    - [数据库](#数据库)
    - [系统编程](#系统编程)
    - [网络编程](#网络编程)

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

#### 大端vs小端

##### 什么是字节序

字节顺序，又称端序或尾序（英语：Endianness），在计算机科学领域中，指电脑内存中或在数字通信链路中，组成多字节的字的字节的排列顺序。

在几乎所有的机器上，多字节对象都被存储为连续的字节序列。例如在C语言中，一个类型为int的变量x地址为0x100，那么其对应地址表达式&x的值为0x100。且x的四个字节将被存储在电脑内存的0x100, 0x101, 0x102, 0x103位置。

字节的排列方式有两个通用规则。例如，将一个多位数的低位放在较小的地址处，高位放在较大的地址处，则称小端序；反之则称大端序。在网络应用中，字节序是一个必须被考虑的因素，因为不同机器类型可能采用不同标准的字节序，所以均按照网络标准转化。

例如假设上述变量x类型为int，位于地址0x100处，它的值为0x01234567，地址范围为0x100~0x103字节，其内部排列顺序依赖于机器的类型。大端法从首位开始将是：0x100: 0x01, 0x101: 0x23,..。而小端法将是：0x100: 0x67, 0x101: 0x45,..。

##### 大端序

![大端序](imgs/image.png)

低位高地址

##### 小端序

![小端序](imgs/image-1.png)

低位低地址

> tips: 为了保证传送顺序的一致性, 网际协议使用大端字节序来传送数据。

**参考**

[维基百科-字节序](https://zh.wikipedia.org/zh-cn/%E5%AD%97%E8%8A%82%E5%BA%8F)

### C语言
---

#### 函数参数入栈顺序及i++,++i实现

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

#### 函数指针 && 函数指针数组 && 指向函数指针数组的指针

##### 函数指针

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

##### 函数指针数组

声明一个数组，里面存储的类型是指向函数的指针

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

##### 函数指针数组

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
1) 转化按数据长度增加的方向进行，以保证精度不降低。例如int类型和long类型运算时，先把int类型转换成long类型后再进行运算
2) 即当参加算数或比较运算的两个操作数类型不统一时，将简单类型向复杂类型转换

$$char->short->int->long->float->double$$

3) 在赋值语句中，赋值号两边数据类型一定是相兼容的类型，如果等号两边数据类型不兼容，语句在编译时会报错

#### 设置、清除、切换和检查单个位

设置第N位：`Number |= (1ul << nth Position)`

设置第N位意味着如果第N位为0，则将其设置为1，如果为1，则保持不变。在C中，按为或运算用于设置整数数据类型的位。据我们所知 | 计算一个新的整数值，其中每个位的位置只有当操作数(数据类型)在该位置为1时才为1。

简而言之，如果其中任何一位为1，则可以说两位的按位与始终为1

0 | 0 = 0  
1 | 0 = 1  
0 | 1 = 1  
1 | 1 = 1  

清除位：`Number &= ~(1UL << nth Position)`

清除位意味着如果第N位为1，则将其清为0，如果为0，则保持不变。按位与运算符用于清除位整数数据类型。如果其中任何一位为零，则两位的“与”始终为零。

0 & 0 = 0  
1 & 0 = 0  
0 & 1 = 1  

检查位:`Bit = Number & (1UL << nth Position)`

要检查第n位，先将第n个“1” 位置向左移动，然后将其与数字“与”

切换位：`Numebr ^ = (1UL << nth Position)`

切换位表示如果第N位为1，则将其更改为0， 如果为0， 则将其更改为1。按位异或运算符用于切换整数数据类型的位。要切换第n个位移位，将第n个位置的'1'向左移动并异或它

0 ^ 0 = 1  
1 ^ 0 = 1  
0 ^ 1 = 0  
1 ^ 1 = 1  

#### 空指针 && 野指针

##### 空指针

指向地址0的指针

C语言中

```c
#define NULL ((void *)0) // msvc
int *p = NULL;
```

C++

```cpp
#define NULL 0 // msvc
```

C++11中加入`nullptr`

NULL 与 nullptr比较

```cpp
void func(int n); 
void func(char *s);
 
func( NULL );
```

使用如上的函数重载时，在调用`func( NULL )`时，我们期望`void func(char *s);`被调用，而实际上`NULL`被解释为0，因此编译器将调用`void func(int n);`

在 C++11 中，nullptr 是一个新关键字，可以（并且应该！）用于表示 NULL 指针；

**Notice:**

>空指针不能被解引用，类似`int *p = NULL; *p = 2;`的语句会引起编译错误

##### 野指针

定义：

>一个指针既不引用合法对象，也不为 NULL

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


### C++
---

#### STL

##### unordered_set

**Syntax**

```cpp
std::unordered_set<data_type> name;
```

常用函数

* `size()`、`empty()`
* `find()`
* `insert()`
* `erase()`、`clear()`

Example:

```cpp
    unordered_set<int> set;
    vector<int> nums = {1, 1, 2, 4, 5};

    for (const int& it : nums) {
        set.insert(it);
    }

    for (const auto& it : set) {
        cout << it << " ";
    }
    // output: 1 2 4 5

    // erase a element
    set.erase(1);

    //clear the unordered_set
    set.clear();
    set.erase(set.begin(), set.end());
```

Output

```
1 2 4 5
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

时间复杂度：$\mathcal{O(n^2)}$  
空间复杂度：$'O(1)'$  
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

时间复杂度：$O(n^2)$  
空间复杂度：$O(1)$  
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

时间复杂度：$O(n^2)$  
空间复杂度：$O(1)$  
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
排序算法比较
![alt text](imgs/image-2.png)

参考  
[hello 算法](https://www.hello-algo.com/chapter_sorting/summary/#1)

### 数据库
---

### 系统编程
---

### 网络编程
---
