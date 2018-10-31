# 需要解决的疑惑

- 1.引用 & 从一而终问题
- 2.拷贝构造函数 const 


- 3. (a++) = 30  (++a) = 40  ✔️

```
int a = 10;
int c = a++ + ++a + a++ + ++a;
```


++a;
//a = 1;
movl   $0x1, -0x4(%rbp)
//eax = 1;
movl   -0x4(%rbp), %eax
//eax +1 
addl   $0x1, %eax
// a = 2
movl   %eax, -0x4(%rbp)


- 4. 练习Point 运算符重载、

Point p(5,10)

支持的运算符
p++
++p
p--;
+=
-=
<<  打印








