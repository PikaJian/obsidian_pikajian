# C/C++ Strict Alias 小记 - CSDN博客

# **C/C++ Strict Alias 小记**

## 什么是Aliasing？

[理解strict  aliasing](http://cellperformance.beyond3d.com/articles/2006/06/understanding-strict-aliasing.html)一文中这样描述：

* 当两个指针指向同一块区域或对象时，我们称一个指针 alias 另一个指针。

[strict  aliasing](http://dbp-consulting.com/tutorials/StrictAliasing.html)一文中这样描述：

	Aliasing 是指多于一个的**左值**指向同一块区域。
	

比如：

|     |     |     |
| --- | --- | --- |
| int i;<br>int \*pi = &i; | pi alias i |     |
| int i;<br>void foo(int &i1, int &i2){}<br>foo(i,i); | i1 alias i2 |     |
| int i;<br>float \*pf = (float\*)&i;<br>\*pf = 0; | pf alias i | 违反strict alias ? |

## 什么是Strict Aliasing？

按照 [理解strict  aliasing](http://cellperformance.beyond3d.com/articles/2006/06/understanding-strict-aliasing.html) 一文描述：

	Strict aliasing 是C或C++编译器的一种假设：**不同**类型的指针绝对不会指向同一块内存区域。
	

### 例子

暂且不管这句话，我们看个例子：

* hello.c

```
#include <stdio.h>
int a;
int f(float *b)
{
    a = 1;
    *b = 0;
    return a;
}
int main()
{
    printf("%d\n", f((float*)&a));
    return 0;
}
```

用GCC4.4 加 -O3 编译(GCC4.5似乎看不到这个效果，fixme)：
```
debao@ubuntu:~/ttt$ gcc-4.4 -Wall -O3 hello.c
hello.c: In function ‘main’:
hello.c:7: warning: dereferencing pointer ‘a.16’ does break strict-aliasing rules
hello.c:13: note: initialized from here
```

运行程序，结果为 1
```
debao@ubuntu:~/ttt$ ./a.out
1
```

而如果不加 -O3，程序结果为 0
```
debao@ubuntu:~/ttt$ gcc-4.4 -Wall  hello.c
debao@ubuntu:~/ttt$ ./a.out
0
```

### 原因

警告信息说：对指针a.16的解引用打破了 strict-aliasing 规则。
```
int a;
int f(float *b)
{
    a = 1;
    *b = 0;
    return a;
}
```

按照strict aliasing规则，编译器认为 a 和 \*b 绝不会指向同一块存储区域，故而优化后返回a时直接返回了1。
那么哪些 alias 是不会破坏规则的呢？或者说规则在哪儿呢？

### 允许的alias

C/C++ 中相应的标准原文见下一节。[strict  aliasing](http://dbp-consulting.com/tutorials/StrictAliasing.html)一文中将这些条文可以总结如下：

* 兼容类型(指相同类型？)或差别仅在于signed、unsigned、const、volatile的类型（比如 const unsigned long \*和 long\*）
* 聚合类型(struct或class)或联合类型(union)可以alias它们所包含的类型（比如 int 和 包含有int的结构体(包括间接包含)）
* 字符类型(char \*、signed char\*、unsinged char\*)可以 alias 任何类型的指针
* \[C++\] 基类的类型(可能带有const、volatile等cv修饰)可以alias派生类的类型

## 为什么要strict alias?

主要目的应该就是为了使编译器能生成更高效的代码。
考虑下面的代码：
```
int a;
void f( double * b )
{
a = 1;
*b = 2.0;
g(a);
}
```

如果没有strict alias假定，编译器必须始终假设b可能会指向a所在的地址，从而不能将g(a)调用优化成g(1)
strict alias规则中，为什么允许char类型可以alias任何对象呢？

* Character pointer types are often used in the bytewise manipulation of objects;a byte stored through such a character pointer may well end up in an object of any type.

为什么class、struct、union可以 alias 它们包含的对象类型呢？

* Structure and union types also have problematic aliasing properties:
	

### 乱七八糟

这可能是一个很常用的东西：比如一个union fi{float f;int i;}对象，我们可以用f成员保存一个浮点数进去，用i成员读一个整数出来。这在C99以及C1X标准中都是明文允许的。

* If the member used to read the contents of a union object is not the same as the member last used to store a value in the object, the appropriate part of the object representation of the value is reinterpreted as an object representation  in the new type as described in 6.2.6 (a process sometimes called ‘‘type punning’’). This might be a trap representation.

可是，在C++标准中却没有类似的条文，尽管C++编译器几乎都支持这种用法，可能这个在C++中仍属于未定义行为。

## C/C++标准条文

* C89 (6.3，我没找到C89的标准原文，对此不确定)
* C99 (6.5/7)
* C++98 (3.10/15)
* C++0x (3.10/10)

都有如下的表述：**如果使用不符合下表中条文所列类型的 alias 来访问一个对象，其行为是为定义的！！！**

* If a program attempts to access the stored value of an object through a glvalue of other than one of the following types the behavior is undefined

|     |     |     |
| --- | --- | --- |
|     | **条文** | **标准** |
| 1   | the dynamic type of the object, | C++0x, C++98 |
| a type compatible with the effective type of the object, | C99 |
| 2   | a cv-qualified version of the dynamic type of the object, | C++0x, C++98 |
| a type similar to the dynamic type of the object, | C++0x, |
| a qualified version of a type compatible with the effective type of the object, | C99 |
| 3   | a type that is the signed or unsigned type corresponding to the dynamic type of the object, | C++0x, C++98 |
| a type that is the signed or unsigned type corresponding to the effective type of the object, | C99 |
| 4   | a type that is the signed or unsigned type corresponding to a cv-qualified version of the dynamic type of the object, | C++0x, C++98 |
| a type that is the signed or unsigned type corresponding to a qualified version of the effective type of the object, | C99 |
| 5   | an aggregate or union type that includes one of the aforementioned types among its elements or nonstatic data members (including, recursively, an element or non-static data member of a subaggregate or contained union), | C++0x, C++98 |
| an aggregate or union type that includes one of the aforementioned types among its members (including, recursively, a member of a subaggregate or contained union), or | C99 |
| 6   | a type that is a (possibly cv-qualified) base class type of the dynamic type of the object, | C++0x, C++98 |
| 7   | a char or unsigned char type. | C++0x, C++98 |
| a character type. | C99 |

## 参考

	<http://cellperformance.beyond3d.com/articles/2006/06/understanding-strict-aliasing.html>
	
	<http://en.wikipedia.org/wiki/Aliasing_%28computing%29>
	
	<http://en.wikipedia.org/wiki/Type_punning>
	
	<http://stackoverflow.com/questions/98650/what-is-the-strict-aliasing-rule>
	
	<http://dbp-consulting.com/tutorials/StrictAliasing.html>
