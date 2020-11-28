# c初探：数据类型、数组、内存布局、指针

> **windows命令行 (可以不用搭理这里，在Linux玩就行)**
>
> Windows C/C++编译器： https://sourceforge.net/projects/mingw/files/
>
> ![mingw](mingw.png)
>
> 配置环境变量 PATH: ${MinGW安装目录}/MinGW/bin
>
> ![环境变量](环境变量.png)



> c与c++
>
>   C语言是一门通用计算机编程语言，广泛应用于底层开发。
>
>   c语句是面向过程的语言，c++是面向对象的语言，C++对c进行扩展。
>
>   c是c++的子集,c++是c的超集，所以大部c语言程序都可以不加修改的拿到c++下使用。

## 1、基本数据类型

 	1.**signed**----有符号，可修饰char、int。Int是默认有符号的。
	2.**unsigned**-----无符号，修饰int 、char



| 整型           | 字节 | 取值范围                        | 占位 |
| :------------- | ---- | ------------------------------- | ---- |
| int            | 4    | -2,147,483,648 到 2,147,483,647 | %d   |
| unsigned int   | 4    | 0 到 4,294,967,295              | %u   |
| short          | 2    | -32,768 到 32,767               | %hd  |
| unsigned short | 2    | 0 到 65,535                     | %hu  |
| long           | 4    | -2,147,483,648 到 2,147,483,647 | %ld  |
| unsigned long  | 4    | 0 到 4,294,967,295              | %lu  |
| char           | 1    | -128 到 127                     | %c   |
| unsigned char  | 1    | 0 到 255                        | %c   |

> 为了得到某个类型或某个变量在特定平台上的准确大小，使用 sizeof 运算符。
>
> 表达式 sizeof(type) 得到对象或类型的存储字节大小。

long int 其实就是长整型 = long 可以省去int
在标准中,规定 int至少和short一样长，long至少和int一样长。

> 为什么会存在long?
>
> long和int在早期16位电脑时候 int 2字节，long 4字节，而计算机发展到现在，一般32、64下，long和int一样。和java类比的话，java的long就是 long long 8字节

> 格式化还有： 
>
> 8进制 			%o
> 16进制			小写： %x    大写：%X
> (0x)+16进制前面 	%#x 



| 浮点型      | 字节 | 精度     | 占位 |
| ----------- | ---- | -------- | ---- |
| float       | 4    | 6位小数  | %f   |
| double      | 8    | 15位小数 | %lf  |
| long double | 8    | 19位小数 | %Lf  |



C99标准以前，C语言里面是没有bool，C++里面才有，
C99标准里面定义了bool类型，需要引入头文件stdbool.h
bool类型有只有两个值：true =1 、false=0。
因此实际上bool就是一个int
所以在c/c++中 if 遵循一个规则， 非0为true，非空为true；
NULL 其实也就是被define为了 0 



## 2、格式化

#include <stdio.h>

printf、sprintf等

sprintf:

​	将格式化的数据写入第一个参数

```c
char str[100];
sprintf(str, "img/png_%d.png", 1);
printf("%s", str);

//使用 0 补到3个字符
sprintf(str, "img/png_%03d.png", 1);
printf("%s", str);
```





## 3、数组与内存布局

> 数组  ： 连续的内存

```c
//java
int[] a

//c
//必须声明时候确定大小
int a[10]  
//或者 直接初始化 
int a[] = {1,2,3}

//大小
printf("%d",sizeof(a)/sizeof(int));
```

> 栈内存限制  linux：ulimit -a 查看
> 但是直接分配这么大不行，因为堆栈可能保存参数，返回地址等等信息



#### 动态内存申请

**malloc** 
	没有初始化内存的内容,一般调用函数memset来初始化这部分的内存空间.

**calloc**
	申请内存并将初始化内存数据为NULL.

​	 ` int *pn = (int*)calloc(10, sizeof(int));`

**realloc**

​	 对malloc申请的内存进行大小的调整.

```c
char *a = (char*)malloc(10);
realloc(a,20);
```

特别的：
**alloca**
	在栈申请内存,因此无需释放.
`int *p = (int *)alloca(sizeof(int) * 10);`



**物理内存**
	物理内存指通过物理内存条而获得的内存空间

**虚拟内存**
	一种内存管理技术
	电脑中所运行的程序均需经由内存执行，若执行的程序占用内存很大，则会导致内存消耗殆尽。
	虚拟内存技术还会匀出一部分硬盘空间来充当内存使用。



![](内存布局.png)

![内存布局解释](内存布局解释.jpg)







代码段：
存放程序执行代码（cpu要执行的指令）

> 栈是向低地址扩展数据结构
> 堆是向高地址扩展数据结构

进程分配内存主要由两个系统调用完成：**brk和mmap** 。

1. brk是将_edata(指带堆位置的指针)往高地址推；
2. mmap 找一块空闲的虚拟内存。



通过glibc (C标准库)中提供的malloc函数完成内存申请

malloc小于128k的内存，使用brk分配内存，将_edata往高地址推,大于128k则使用mmap

![brk申请内存](brk申请内存.png)




# 指针、函数、预处理器

[TOC]



## 1、指针

> 指针是一个变量，其值为地址。
>
> 声明指针或者不再使用后都要将其置为0 (NULL)
>
> 野指针    	未初始化的指针
>
> 悬空指针  	指针最初指向的内存已经被释放了的一种指针

```c
int *a; 正规
int* a;
int * a;
//因为 其他写法看起来有歧义
int* a,b;
```

使用：

```c
//声明一个整型变量
int i = 10;
//将i的地址使用取地址符给p指针
int *p = &i;
//输出 0xffff 16进制地址
printf("%#x\n", &i);
printf("%#x\n", &p);
```

> 指针多少个字节？指向地址，存放的是地址 
>
> 地址在 32位中指针占用4字节 64为8

```c
//32位：
sizeof(p) == 4;
//64位:
sizeof(p) == 8;
```

#### 解引用

> 解析并返回内存地址中保存的值 

```c
int i = 10;
int *p = &i;
//解引用  
//p指向一个内存地址，使用*解出这个地址的值 即为 10
int pv = *p;
//修改地址的值,则i值也变成100
//为解引用的结果赋值也就是为指针所指的内存赋值
*p = 100;
```

#### 指针运算

```c
//对指针 进行算数运算
//数组是一块连续内存 分别保存 11-55
//*p1 指向第一个数据 11，移动指针就指向第二个了
int i1[] = {11,22,33,44,55};
int *p1 = i1;
for (size_t i = 0; i < 5; i++)
{
    //自增++ 运算符比 解引用* 高,但++在后为先用后加
    //如果++在前则输出 22-55+xx
	printf("%d\n", *p1++);
    //p1[0] == *(p1+1) == s[1]

}
```

> 指针指向地址，指针运算实际上就是移动指针指向的地址位置,移动的位数取决于指针类型（int就是32位）

![指针移动](指针移动.png)

#### 数组和指针

> 在c语言中，指针和数组名都表示地址
>
> 1、数组是一块内存连续的数据。 
>
> 2、指针是一个指向内存空间的变量 

```c
int i1[] = {11,22,33,44,55};
//直接输出数组名会得到数组首元素的地址
printf("%#x\n",i1);
//解引用
printf("%d\n",*i1);
//将数组名赋值给一个指针，这时候指针指向数组首元素地址
int *p1 = i1;

//数组指针
//二维数组类型是 int (*p)[x]
int array[2][3] = { {11,22,33},{44,55,66} };
//也可以 int array[2][3] = {  11,22,33 ,44,55,66 };
//array1 就是一个 int[3] 类型的指针
int (*array1)[3] = array;
//怎么取 55 ？
//通过下标
array[1][1] == array1[1][1]
//通过解引用
int i = *(*(array1 + 1) + 1);
//拆分
//1、 指针偏移 因为array1的类型是3个int的数组 所以 +1 移动了12位
array1 + 1 
//2、获得{44,55,66}数组  (*(array1 + 1))[1] = 55
(*(array1 + 1) 
//3、对数组执行 +/- 相当于隐式的转为指针
//获得 55 的地址 再解地址
*(array1 + 1) + 1
 
 
//指针数组
int *array2[2];
array2[0] = &i;
array2[1] = &j;
```



#### const char *, char const *, char * const，char const * const 

> const ：常量 = final

```c
//从右往左读
//P是一个指针 指向 const char类型
char str[] = "hello";
const char *p = str;
str[0] = 'c'; //正确
p[0] = 'c';   //错误 不能通过指针修改 const char

//可以修改指针指向的数据 
//意思就是 p 原来 指向david,
//不能修改david爱去天之道的属性，
//但是可以让p指向lance，lance不去天之道的。
p = "12345";

//性质和 const char * 一样
char const *p1;

//p2是一个const指针 指向char类型数据
char * const p2 = str;
p2[0] = 'd';  //正确
p2 = "12345"; //错误

//p3是一个const的指针变量 意味着不能修改它的指向
//同时指向一个 const char 类型 意味着不能修改它指向的字符
//集合了 const char * 与  char * const
char const* const p3 = str;
```



#### 多级指针

> 指向指针的指针
>
> 一个指针包含一个变量的地址。当我们定义一个指向指针的指针时，第一个指针包含了第二个指针的地址，第二个指针指向包含实际值的位置。

```c
int a = 10;
int *i = &a;
int **j = &i;
// *j 解出 i   
printf("%d\n", **j);
```

##### 多级指针的意义

> 见函数部分的引用传值



## 2、函数

> C中的函数与java没有区别。都是一组一起执行一个任务的语句，也都由 **函数头**与**函数体**构成

#### 函数的位置

> 声明在使用之前

#### 函数参数

##### 	传值调用

​		把参数的值复制给函数的形式参数。修改形参不会影响实参

##### 	引用调用

​		形参为指向实参地址的指针，可以通过指针修改实参。

```c
void change1(int *i) {
	*i = 10;
}
void change2(int *i) {
	*i = 10;
}
int i = 1;
change1(i);
printf("%d\n",i); //i == 1
change2(&i);
printf("%d\n",i); //i == 10
```

#### 可变参数

> 与Java一样，C当中也有可变参数

```c
#include <stdarg.h>
int add(int num, ...)
{
	va_list valist;
	int sum = 0;
	// 初始化  valist指向第一个可变参数 (...)
	va_start(valist, num);
	for (size_t i = 0; i < num; i++)
	{
		//访问所有赋给 valist 的参数
		int j = va_arg(valist, int);
		printf("%d\n", j);
		sum += j;
	}
	//清理为 valist 内存
	va_end(valist);
	return sum;
}
```



#### 函数指针

> 函数指针是指向函数的指针变量

```c
void println(char *buffer) {
	printf("%s\n", buffer);
}
//接受一个函数作为参数
void say(void(*p)(char*), char *buffer) {
	p(buffer);
}
void(*p)(char*) = println;
p("hello");
//传递参数
say(println, "hello");

//typedef 创建别名 由编译器执行解释
//typedef unsigned char u_char;
typedef void(*Fun)(char *);
Fun fun = println;
fun("hello");
say(fun, "hello");

//类似java的回调函数
typedef void(*Callback)(int);

void test(Callback callback) {
	callback("成功");
	callback("失败");
}
void callback(char *msg) {
	printf("%s\n", msg);
}

test(callback);
```

## 3、预处理器

> 预处理器不是编译器，但是它是编译过程中一个单独的步骤。
>
> 预处理器是一个文本替换工具
>
> 所有的预处理器命令都是以井号（#）开头

#### 常用预处理器

| 预处理器 | 说明         |
| -------- | ------------ |
| #include | 导入头文件   |
| #if      | if           |
| #elif    | else if      |
| #else    | else         |
| #endif   | 结束 if      |
| #define  | 宏定义       |
| #ifdef   | 如果定义了宏 |
| #ifndef  | 如果未定义宏 |
| #undef   | 取消宏定义   |

#### 宏

**预处理器是一个文本替换工具**

> 宏就是文本替换

```c
//宏一般使用大写区分
//宏变量
//在代码中使用 A 就会被替换为1
#define A 1
//宏函数
#defind test(i) i > 10 ? 1: 0

//其他技巧
// # 连接符 连接两个符号组成新符号
#define DN_INT(arg) int dn_ ## arg
DN_INT(i) = 10;
dn_i = 100;

// \ 换行符
#define PRINT_I(arg) if(arg) { \
 printf("%d\n",arg); \
 }
PRINT_I(dn_i);

//可变宏
#define LOGE(...) __android_log_print(ANDROID_LOG_ERROR,"NDK", __VA_ARGS__);

//陷阱
#define MULTI(x,y)  x*y
//获得 4
printf("%d\n", MULTI(2, 2));
//获得 1+1*2  = 3
printf("%d\n", MULTI(1+1, 2));
```

> 宏函数
>
> ​	优点：
>
> ​		文本替换，每个使用到的地方都会替换为宏定义。
>
> ​		不会造成函数调用的开销（开辟栈空间，记录返回地址，将形参压栈，从函数返回还要释放堆		
>
> ​		栈。）
>
> ​	缺点：
>
> ​		生成的目标文件大，不会执行代码检查
>
> 
>
> 内联函数
>
> ​	和宏函数工作模式相似，但是两个不同的概念，首先是函数，那么就会有类型检查同时也可以debug
> 在编译时候将内联函数插入。
>
> 不能包含复杂的控制语句，while、switch，并且内联函数本身不能直接调用自身。
> 如果内联函数的函数体过大，编译器会自动的把这个内联函数变成普通函数。


