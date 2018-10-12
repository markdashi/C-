#  C++

## 1、函数重载（Overload）
### 规则
- [x] 函数名相同
- [x] 参数和属不同、参数类型不同、参数顺序不同

###  **注意**
- [x] 返回值类型与函数重载无关
- [x] 调用函数时，实参的隐式类型转换可能产生二义性

### **本质**
- [x] 采用了name mangling 或者叫name decoration技术

✔️ C++编译器默认会对符号名(变量名、函数名)进行改编，修饰

✔️重载是会生成多个不同的函数名，不同编译器（MSVC g++）有不同的生成规则

✔️通过IDA打开【VS_Release_禁止优化】可以看到

例一:

```
diplayer_int
void diplayer(int a){
cout << "diplayer(int a) << " << a << endl;
}
diplayer_double
void diplayer(double a){
cout << "diplayer(double a) << " << a << endl;
}
diplayer_long
void diplayer(long a){
cout << "diplayer(long a) << " << a << endl;
}
```

例二:

```
int add(int a,double b){
return a + b;
}
int add(double a,int b){
return a + b;
}
```

## 2.extern "C"
C 与C++ 混合开发

被 extern “C” 修饰的代码会按照C语言的方式去编译

- C++ 在调用C语言API时，需要使用extern “C”修饰C语言函数声明

```
extern "C"{
 
}
```
### 默认参数

 C++ 允许函数设置默认参数，在调用时可以根据情况省略实参。规则如下:
- [x] 默认参数只能按照右到左顺序
- [x] 如果函数同时有声明、实现、默认参数只能放在函数声明中 
- [x]  默认参数的值可以是常量、全局符号（全局变量、函数名）
- [x] 函数重载、默认参数可能产生冲突、二义性（建议优先选择默认参数）

例一：
```
void func(int a,int b, int c = 20){

}
func(10,20);
```
例二：
```
int age = 70;
void func(int a,int b, int c = age){

}

```

例三:

```
void display(){
    cout << "display" << endl;
}
void display(int a = 10,int b = 20){
    cout << "a is" << a << endl;
    cout << "a is" << b << endl;
}

// 产生二义性
display();

```


指向函数的指针
```
void test(){

}

void (*funcPtr)() = test;
funcPtr();
```
## 3.内联函数（inline function）

使用`inline`修饰函数的声明或者实现，可以使其变成内联函数
- [x]建议声明和实现都增加`inline`修饰

### 特点
- [x] 编译器会将函数调用直接展开为函数体代码
- [x] 可以减少函数调用的开销
- [x] 会增大代码体积

### 注意
- [x] 尽量不要内联代码超过10行的函数
- [x] 有些函数即使声明为`inline`,也不一定会被编译器内联，比如递归函数

- 代码量不是很多
- 函数调用频率很高

### 内联函数和宏

- [x] 内联函数和宏都可以减少函数调用的开销
- [x] 对比宏，内联函数多了语法检测和函数特性

### 思考以下代码的区别
- `#define sum(x) (x+x)`
- `inline int sum(int x) {return x+x}`
- `int a = 10; sum(a++);`

### #pragma once

- 我们经常使用 `#ifndef`、`#define`、`#endif`来防止头文件的内容被重复包含

- `#pragma once` 可以防止整个文件的内容被重复包含

### 区别
- `#ifndef`、`#define`、`#endif` 受C\C++标准的支持，不受编译器的任何限制

- 有些编译器不支持 `#pragma once`，兼容性不好

- `#ifndef`、`#define`、`#endif` 可以针对文件中的部分代码，而 `#pragma once`只能针对整个文件


## 4.引用（Reference）

- 在C语言中，使用指针（Pointer）可以间接获取、改变某个变量的值
- 在C++中，使用引用（Reference）可以起到跟指针类似的功能

```
int a = 20;
// rage就是一个引用/定义了一个引用
// 定义了一个引用，相当于变量的别名
int &rage = age;
```
### 注意点
- 引用相当于是变量的的别名（基本数据类型、枚举、结构体、类、指针、数组等，都可以有引用）
- 对引用做计算，就是对引用所指的变量做计算
- 在定义的时候必须初始化，一旦指向了某个变量，就不可以再改变，“从一而终”
- 可以利用引用初始化另一个引用，相当于某个变量的多个命名
- 不存在【引用的引用，指向引用的指针，引用数组】

**引用存在的价值之一：比指针更安全，函数返回值可以被赋值** 

```
int age = 10;
int &rAge = age;
//1.不存在引用的引用
int &&rAge2 = rAge;
//2.不存在指向引用的指针
int &*p = &rAge;
//3.不存在引用数组
int &array[4]
```


1.指针引用
```
int a = 10;
int b = 20;

int *p = &a;
int *&rp = p;
rp = &b;
*p = 30;

a = 10, b = 30
```

2.数组引用
```
int array[] = { 10, 20, 30};
int (&rArray)[3] = array;


int *a[4];//指针数组
int (*b)[4];//指向数组的指针

```
使用场景

值交换
```
void swap(int &a,int &b){
    int temp = a;
    a = b;
    b = temp;
}

int v1 = 10;
int v2 = 20;
swap(v1,v2);

```

### const

 const 是常量的意思，被修饰的变量不可以更改

- 如果修饰的是类、结构体（的指针）其成员变量不可修改

```
struct Student{
  int age
}

Student stu = {20};

//不能修改age的值
const Student *pStu = &stu;
pStu->age = 30;

```

例子：
以下5个指针分别是什么含义
```
int age = 10;
const int *p0 = &age;
int const *p1 = &age;
int * const p2= &age;
```




