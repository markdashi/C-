#  C++

### 2018-10-10
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

## extern "C"

C 与C++ 混合开发

被 extern “C” 修饰的代码会按照C语言的方式去编译

- C++ 在调用C语言API时，需要使用extern “C”修饰C语言函数声明

```
extern "C"{
 
}
```
## 默认参数

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
















