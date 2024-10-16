---
layout: post
title: C语言基础学习笔记
subtitle: 初级阶段总结
date: 2024-10-02 15:56:00
author: pqqqYa
header-img: img/post/2024-10-02-CNote-bg.jpg
catalog: true
tags:
  - 笔记
  - C
  - Cpp
---

  

> 文章为王道C语言督学训练营初级阶段（C语言入门）网课笔记整理。
  
 
 
# 1. 入门
 
## 1.1 程序的作用是么？
 
 帮我们完成某种计算

## 1.2 文件结构

~~~c
#include <stdio.h>              //头文件
int main(){                     //入口函数main
	printf("hello world!\n");   //函数内得代码内容
	retrun 0;                   //函数得返回值
}
~~~

## 1.3 项目的编译

首先编写源程序 main.c。编写完毕后，通过编译器进行编译，main.c经过编译后，得到可执行文件(windows下是 exe，Mac 和Linux 下是不带后缀的，统称为可执行文件)，可执行文件中均是 0/1 类型的机器码，即 CPU 能够识别的微指令(英特尔的机器指令)，CPU 才能够去执行。

## 1.4 断点调试

* 添加断点，让程序运行慢下来，一点一点的运行。
* 通过变量监视窗口发现某个变量的值不符合预期，说明出现了BUG。


# 2. 数据类型

## 2.1 数据类型

### 2.1.1 基本数据类型

* 基本类型
	* 数值类型
		* 整形
			* 整型int（4个字节）
			* 短整型short（2个字节）
			* 长整型long（4个字节）
	* 浮点型
		* 单精度型float（4个字节）
		* 双精度型double（8个字节）
	* 字符型char（1个字节）
* 构造类型
	* 数组`[ ]`
	* 结构体struct
	* 共用体union
	* 枚举类型enum
* 指针类型 `*`
* 空白类型void


### 2.1.2 常量 

在程序运行过程中值不发生变化的值
* 整型
* 实型（浮点型）
* 字符型
* 字符串型（一般不会考汉字，用汉字需要切换编码）

### 2.1.3 变量

* 在程序运行过程中值可以发生改变的值。
* 变量的实质是一个名字代表一个对应的存储单元地址。编译、链接程序的时候，由编译系统为每个变量分配对应的内存地址。
* 变量的命名：标识符只能由**字母**、**数字**和**下画线**三种字符组成，并且第一个字符必须为字母或下画线。
* 见名知意
* 不能与关键字同名

### 2.1.4 整型数据
#### 2.1.4.1 符号常量

define定义完成后无法在函数里面重新定义

~~~c
#include <stdio.h>  
#define PI 3+2  
int main()  
{  
    int i = PI*2;  
    printf("i = %d\n",i);  
    return 0;  
}
//运行结果 i = 7 (3+2*2),是直接移下来的，不进行运算
~~~

#### 2.1.4.2 整型变量

变量int i 的大小是4个字节
~~~c
#include <stdio.h>  
int main()  
{  
    int i=10;//设置整型变量i  
    printf("i = %d\n",i);//输出i的值  
    printf("i size= %d byte \n",sizeof(i));//输出i的字节数  
    return 0;  
}
//输出结果：
//i = 10
//i size= 4 byte
~~~

### 2.1.5 浮点型数据
#### 2.1.5.1 浮点型常量

* 小数形式：0.003
* 指数形式：3e-3（$3 × 10^{-3}$）
* 注意：在使用的时候区分数学的e，且e的前面只能是整数且后面不为空的整数
#### 2.1.5.2 浮点型变量

### 2.1.6 字符型数据

#### 2.1.6.1 字符型常量

* 在单引号中的一个字母`'A'`
* 转义符
	* `\n`换行
	* `\b`退格
	* `\\`换行符

~~~c
#include <stdio.h>  
int main()  
{
    char c = 'A';  
    printf("%c\n",c+32);//以字符的形式输出  
    printf("%d\n",c+32);//以ASCII码（数值）的形式输出  
}
//输出：
//a
//97
~~~

### 2.1.7 字符串型常量
* 在双引号中的一个或者多个字母`"A"`或者`"Abc"`
* 每一个字符串都有结尾的`\0`，比如：`"CHINA"`存储的是`CHINA\0`占用6个字节

## 2.2 混合运算
### 2.2.1 混合运算 
类型强制转换的场景
~~~c
#include <stdio.h>  
//强制类型转换  
int main()  
{  
    int i = 5;  
    float f = i/2;
    //这里做的是整型运算，因为左右操作数都是整型，所以结果是2  
    float f2 =(float)i/2;
    //这里做的是浮点型运算，因为左边操作数是浮点型，所以结果是2.5  
    printf("%f\n", f);  
    printf("%f\n", f2);  
}
//输出结果：
//2.000000
//2.500000
~~~
### 2.2.2 printf函数
printf函数将这些类型的数据格式化为字符串后，放入标准输出缓冲区，让后将结果输出到屏幕上

~~~C
#include <stdio.h> 
int printf(const char *format,.....)
~~~

#### 2.2.2.1 printf的具体代码格式：
* %c字符
* %d带符号的整数
* %f浮点数
* %s一串字符
* %u无符号整数
* %x无符号十六进制数，用小写字母
* %X无符号十六进制数，用大写字母
* %o输出八进制数
* %p一个指针

#### 2.2.2.2 格式控制
~~~c
#include <stdio.h>  
int main()  
{  
    float a= 123.123;  
    printf("%7.2f\n",a);  
    printf("%-7.2f\n",a);  
    int b=123;  
    printf("%4d",b);  
}
~~~
在f或者d前面加数字
* 正表示右对齐（默认），负表示左对齐
* `.`前面的表示占多少个位置
* `.`后面的表示占多少个保留多少位小数（浮点数类型适用）

## 2.3 整型进制转换

### 2.3.1 整型常量的不同进制表示

* 二进制
	* 一个字节为8位（1byte=8bit），1位即二进制的1位，它存储0或1，eg：`0101 0101`
	* 1KB=1024byte
	* 1MB=1024KB
	* 1G=1025MB
* 十进制 0-9
* 八进制0-7
* 十六进制0-9，a-f

进制转换
* 常用的是二进制转换成16进制（每4位二进制数转换成16进制数，`0111 1011`→`7b`）
* 二进制转换成8进制（每3位二进制数转换成8进制数，`001 111 011`→`173`）
~~~c
#include <stdio.h>  
int main()  
{  
    int i=123;//赋值十进制数  
    int i=0173;//赋值八进制数  
    int i=0x7b;//赋值十六进制数  
    printf("%d\n",i);//十进制输出  
    printf("%o\n",i);//八进制输出  
    printf("%x\n",i);//十六进制输出  
    return 0;  
}
~~~

tips：
* 使用16进制刚好2个字符显示2个字节`0111 1011`→`7b`
* 使用小端存储，低字节放在前面
* 在cmd输入calc快速进入计算器（程序员模式快速进制转换）

## 2.4 scanf读取标准输入

### 2.4.1 scanf函数的原理

C语言未提供输入/输出关键字，其输入和输出是通过标准函数库来实现的。C语言通过scanf 函数读取键盘输入，键盘输入又被称为标准输入。当 scanf 函数读取标准输入时，如果没有输入，就会被阻塞。

scanf读取标准输入，scanf把标准输入放到某个变量空间中，因此变量必须取地址
~~~c
#include <stdio.h>  
int main()  
{  
    int i;  
    scanf("%d",&i);
    printf("%d",i);  
}

#include <stdio.h>  
int main()  
{  
    char c;  
    scanf("%c",&c);
    printf("%c",c);  
}

#include <stdio.h>  
int main()  
{  
float f;  
    scanf("%f",&f); 
    printf("%f",f);  
}


~~~

<p style="color:red">错误演示</p>

~~~c
#include <stdio.h>  
int main()  
{  
    int i;  
    char c;  
    scanf("%d",&i);
    printf("i = %d\n",i);  
    scanf("%c",&c);  
    printf("c = %c\n",c);  
    return 0;  
}
~~~

在这里输入“10”回车，输出的是

~~~cmd
10
i = 10
c =


进程已结束，退出代码为 0
~~~

在这里为什么第二个scanf”没有被使用“，因为在标准输入缓冲区其实输入的是`10\n`第一个scanf输入只会读取`10`，会把`\n`留在标准输入缓冲区，直接被char c给存入了。

改正，添加`fflush`清空标准输入缓冲区
~~~c
#include <stdio.h>  
int main()  
{  
    int i;  
    char c;  
    scanf("%d",&i);//注意一定要取地址  
    printf("i = %d\n",i);  
    fflush(stdin);
    //清空标准输入缓冲区，否则会把回车\n读入到c中  
    scanf("%c",&c);  
    printf("c = %c\n",c);  
    return 0;  
}
~~~

scanf在读取整型，浮点数，字符串的时候会忽略`\n`，`空格`等字符，但是读取字符类型不会。

### 2.4.2 多种数据类型的混合输入

~~~c
#include <stdio.h>  
int main()  
{  
    int x,ret;  
    char y;  
    ret = scanf("%d %c",&x,&y);  
    printf("%d %c\n",x,y);  
    printf("ret= %d",ret);//scanf匹配成功个数2
return 0;  
}
~~~

scanf函数其实是有返回值得，返回匹配成功的个数

~~~c
scanf("%d%s %c%d%f%s",......)
~~~

* `%c`不能忽略任何字符，所以不能和前面的几个连在一起读取

# 3. 运算符与表达式

## 3.1 算数运算符与关系运算符

### 3.1.1 运算符的分类

1. **算数运算符（`+ - * / %`）**
2. **关系运算符（`> < == >= <= !=`）**
3. **逻辑运算符（`! && ||`）**
4. 位运算符（`<< >> ~ | ^ &`）
5. **赋值运算符（`=及其扩展赋值运算符`）**
6. 条件运算符（`? :`）
7. 逗号运算符（`,`）
8. 指针运算符（`* &`）
9. **求字节运算符（`sizeof`）**
10. **强制类型转换运算符（`(ElemType)`）**
11. 分类运算符（`. ->`）
12. 下标运算符（`[]`）
13. 其他（`如函数调用运算符()`）

### 3.1.2 算数运算符及算数表达式

* 当操作符的两边都是整型数的时候，它就执行整除运算，其他情况下执行浮点运算
### 3.1.3 关系运算符与关系表达式

* `> < == >= <= !=`六个关系运算符的表达式的值只有真（1）和假（0）
* C语言中没有布尔类型。所以0值代表假，1值代表真
* 关系运算符的优先级低于算术运算符  
* 有多个判断时需要配合逻辑运算符，如：`if(3<a && a<10)`不能写成`if(3<a<10)`

### 3.1.4 运算优先级

算数运算符的优先级高于关系运算符，关系运算符的优先级高于逻辑与逻辑或运算符，相同优先级的运算符从左至右进行结合。

![](https://raw.githubusercontent.com/pqqqYa/pqqqYa.github.io/main/img/post/2024-10-02/Operatorprecedence.png)

记住运算优先级，去掉多余的小括号

## 3.2 逻辑运算符与赋值运算符，求字节运算

### 3.2.1 逻辑运算符与逻辑表达式

短路运算（逻辑与和逻辑或）
~~~c
#include <stdio.h>  
int main(){  
    int i = 0;  
    i && printf("1");  
    int j =1;  
    j && printf("2");  
    int x = 0;  
	x || printf("3");  
	int y =1;  
	y || printf("4");
}
//输出结果：
//2
//3
~~~

当i为假时不会进行逻辑与后面的逻辑运算，当x为真时不会进行逻辑或后面的逻辑运算

### 3.2.2 赋值运算符

* 变量才可以作为左值（left operand）
* `a+=3`和`a=a+3`
* `b*=5`和`b=b*5`

### 3.2.3 求字节运算符

sizeof是一个运算符，不像其他运算符是一个符号，sizeof是字母组成的，用于求常量或变量所占用的空间大小。 

# 4. 选择、循环

## 4.1 选择if-else

### 4.1.1 关系表达式与逻辑表达式

注意优先级，详细查看3.1.4

### 4.1.2 if-else语句

* if(表达式)为真(1)，就执行这个语句；反之则不执行。
* if，else if，else实现多分支语句。
* 括号写法注意规范，方便阅读。
~~~c
if(Expression)
{
	Functions
}else{
	Functions
}
~~~

## 4.2 循环while,for,continue,break

### 4.2.1 while循环 
* while(表达式)为真(1)，就继续执行这个语句；反之则跳出执行。
* 注意防止死循环：循环体内趋于假，分号不要写
~~~c
while(Expression){
	loop body
}
~~~

### 4.2.2 for循环
~~~c
for(Expression 1;Expression 2;Expression 3;){
	loop body
}
~~~

1. 先求解表达式1
2. 求解表达式2，若值为真（值为非0）先执行for循环语句中指定的内嵌语句，后执行第3步。若值为假（值为0），则结束循环，转到第5步
3. 求解表达式3
4. 转回执行第2步
5. 循环结束，执行for语句下面的语句
### 4.2.3 comtinue语句

* `comtinue;`的作用为结束本次循环，即跳过循环体中下面尚未执行的语句，接着进行的是下一次循环的判断。
* 在while中使用注意不要跳过使趋近于假的语句否则可能会导致死循环。

### 4.2.4 break语句

`break;`语句的作用是结束整个循环过程，不再判断执行循环的条件是否成立。

# 5. 一维数组与字符串数组
## 5.1 一维数组

### 5.1.1 数组的定义

数组，一组具有相同数据类型的数据的有序集合。

~~~c
//类型说明符 数组名 [常量表达式]
int a[10]
~~~

* 数组名的命名规则和变量名的相同，即遵循标识符命名规则。
* 在定义数组时，需要指定数组中元素的个数，方括号中的常量表达式用来表示元素的个数，即数组长度。
* 常量表达式中可以包含常量和符号常量，但不能包含变量。也就是说，C语言不允许对数组的大小做动态定义，即数组的大小不依赖于程序运行过程中变量的值。（方括号里面直接写死最好，尽管最新的C标准支持这么写）
* 数组名里面存储有数组的首地址，直接可以赋值给指针变量

常见错误示范：
~~~c
float a[0];   //数组大小为0没有意义
int b(2)(3);  //不能使用圆括号
int k=3,a[k]; //不能使用变量说明数组大小
~~~

### 5.1.2 一维数组在内存中的存储

从第0个开始，依次存放从低地址到高地址

~~~c
int a[10]={1,2,3,4,5,6,7,8,9}
~~~

数组初始化的方式：
1. 在定义数组时对数组元素赋值
2. 可以只给一部分元素赋值（其余部分默认为0）
3. 可以全部赋值为0，但必须全部一起赋值为0
4. 在对全部数组元素赋值时，由于数组的个数已经确定，因此可以不指定数组的长度`int a[]={1,2,3}`（不推荐使用）

## 5.2 数组访问越界与数组的传递

### 5.2.1 数组的访问越界

~~~c
#include <stdio.h>  
int main(){  
    int a[5]={1,2,3,4,5};  
    int j =20;  
    int i =10;  
    a[5]=6;//越界访问  
    a[6]=7;//越界访问会造成数据异常  
    printf("%d",i);//i没有修改赋值，但是值发生了改变  
    return 0;  
}
~~~

![](https://raw.githubusercontent.com/pqqqYa/pqqqYa.github.io/main/img/post/2024-10-02/accessviolation.png)

这里的`a[5]`已经超出了数组a的范围，把`06 00 00 00`放入了他不应该存在的位置，若再下一步到`a[6]`，就会把`14 00 00 00`即`i`的内存空间所占据，修改为`07 00 00 00`，printf输出的结果将是7。（只有在书写程序的时候人工的确保不会发生这些错误，编译器不会检查数组下标的引用）

### 5.2.2 数组的传递

数组名传递到子函数后，子函数的形参接收到的是数组的起始地址，因此不能把数组的长度传递给子函数，想要知道长度只有从外面传递进去。

~~~c
#include <stdio.h>  
void show(int a[],int b)  
{  
    int i;  
    // for(i=0;i<sizeof(a)/sizeof(int);i++)  
    for(i=0;i<b;i++)  
    {  
        printf("%3d",a[i]);  
    }  
}  
  
int main(){  
    int a[5]={1,2,3,4,5};  
    int b =sizeof(a)/sizeof(int);  
    show(a,b);  //数组在传递给子函数的时候，长度无法传递过去。
    return 0;  
}
~~~

## 5.3 字符数组与scanf读取字符串

### 5.3.1 字符数组初始化及传递

1. 字符数组的定义方法与前面的一维数组类似
2. 可以对每个字符串单独初始化
3. 可以对整个数组进行初始化
4. 常用的是使用双引号直接写完整的单词

~~~c
#include <stdio.h>  
int main(){  
    char x[3];  
    x[0]='h';  
    x[1]='i';  
    char y[3]={'h','i'};  
    char z[3]="hi";  
    printf("%s\n",z); 
}
~~~

* 注意在最后还有个结束符`\0`的存在，忽略他可能会导致数组越界
* 使用`%s`可以直接输出整个字符数组
* 没有`\0`结束符的存在在最后就会输出乱码，`%s`直到有了结束符才会停止循环输出字符数组。
* 也可以用`while(d[i])`来模拟`%s`输出字符串数组

### 5.3.2 scanf读取字符串

* 在`scanf("%s",str);`中是不需要使用`&`来取地址的，因为字符数组名中存储了数组的地址（指针章节详见）
* 之前的读取会忽略空格，但是`%s`会将空格认为是结束符，停止读取

## 5.4 gets与puts，strlen-strcmp-strcpy

### 5.4.1 gets函数和puts函数


* gets函数的格式：`char *gets(char *str)`
* gets函数从STDIN标准输入读取字符串并把他们加载到str（字符串）中，直到遇到`\n`后，不会存储`\n`，而是将其翻译为空字符`\0`

~~~c
#include <stdio.h>  
int main(){  
    char c[20];  
    gets(c);//gets中放入的是数组名  
    printf("%s",c);  
    return 0;  
}
~~~

* puts函数的格式：`int puts(char *str)`
* puts等价与printf，但是只能字符串
* 里面所放的参数只能是字符数组名

## 5.4.2 str系类字符串操作函数

str 系列字符串操作函数主要包括strlen、strcpy、strcmp、strcat 等。

* **strlen 函数**用于统计字符串长度（循环遍历统计，找到结束符后，循环结束，从而得出字符串长度）
* **strcpy函数**用于将某个字符串复制到字符数组中
* **strcmp 函数**用于比较两个字符串的大小（比较的是ASCII码表的值的大小，若第一个相等，比较第二个，以此类推；str1=str2为0，str1>str2为正数，str1<str2为负数）
* **strcat 函数**用于将两个字符串连接到一起。

~~~c
#include <string.h>  
size_t strlen(char *str);  
char *strcpy(char *to,const  char *from);  
int strcmp(const char *s1,const char *s2);  
char *strcat(char *str1,const char *str2);
~~~

对于传参类型`char *`，直接放入字符数组的数组名即可。

~~~c
#include <string.h>  
#include <stdio.h>  
int main()  
{  
    int len,temp;  
    char str1[100] = "hello";  
    char str2[100] = "world";  
    char str3[100];  
    len = strlen(str1);// 求字符串str1长度  
    printf("string length is %d\n", len);  
    printf("%d\n", strcmp(str1, str2));// 比较字符串str1和str2的大小  
    strcat(str1, str2);// 连接字符串str1到str2前面  
    printf("%s\n", str1);//打印“helloworld”  
    strcpy(str3, str1);// 复制字符串str1到str3中  
    printf("%s\n", str3);//打印“helloworld”  
}
~~~

# 6. 指针

## 6.1 指针的本质（间接访问原理）

### 6.1.1 指针的定义

内存区域中的每字节都对应一个编号，这个编号就是“地址”。如果在程序中定义了一个变量，那么在对程序进行编译时，系统就会给这个变量分配内存单元。

* 直接访问：按变量地址存取变量值的方式（` printf("&d",i)`；`scanf("&d",&i);`）
* 间接访问：将变量i的地址存放到另一个变量中

~~~c
#include <stdio.h>  
int main()  
{  
    int i = 5;  
    int *i_pointer = &i;//定义一个指针变量  
    printf("i = %d\n", i);//直接访问  
    printf("i_pointer = %d\n", *i_pointer);//间接访问  
    return 0;  
}
~~~

**指针变量**是一种特殊的变量，它用来存放变量地址。

* 定义格式：`基类型 *指针变量名`
* 示例：`int *i_pointer`，注意这里的*i_pointer*才是变量名，没有星号`*`
* 指针变量本身所占的空间大小在在32位程序中占4字节，在64位程序中占8个字节（考研往往强调的是32位程序）
* 指针变量的初始化时某个变量取地址来赋值（必须指定相同的类型，整型变量对应整型指针等）

**指针**可以直接理解为变量的地址的同义词

* 注意区分指针变量
* 如果有一个变量专门用来存放另一变量的地址（即指针），那么称它为"指针变量"


### 6.1.2 取地址操作符与取值操作符

* 取地址操作符为`&`，也称引用，通过该操作符可以获取一个变量的地址值
* 取值操作符为`*`，也称解引用，通过该操作符可以得到一个地址对应的数据
* `&`和`*`的优先级是相同的，但是要按照自右向左的顺序运行


## 6.2 指针的传递使用场景

指针的使用场景通常只有2个，即传递与偏移。

不同函数之间的栈空间是有权限相互访问的

 ~~~c
#include <stdio.h>  
void change(int *j)  
{  
    *j=10;//*j等价于变量i，间接访问变量i  
}  
int main()  
{  
    int i = 5;  
    // int *i_pointer = &i;  
    // change(i_pointer);    
    change(&i);// 传地址，和上面的被注释掉的原理是一样的  
    printf("i = %d\n", i);  
    return 0;  
}
 ~~~

## 6.3 指针的偏移使用场景 

### 6.3.1 指针的偏移

指针的偏移就像地址的加减可以找到相邻的地址，但是对于指针的乘除就像找地址的乘除一样没有意义。  

![](https://raw.githubusercontent.com/pqqqYa/pqqqYa.github.io/main/img/post/2024-10-02/Pointeroffset.png)

* 在这里的第一和第二的输出结果都是一样的是`1 2 3 4 5`，第三个是`5 4 3 2 1`
* 偏移的长度是其类型的长度，也就是偏移sizeof(Elemtype)，这里的p和p+1的地址相差了4

### 6.3.2 指针与一维数组

数组名作为实参传递给子函数时，是弱化为指针的

~~~c
#include <stdio.h>  
void change(char *d)  
{  
    *d = 'H';  
    d[1]='E';  
    *(d+2)='L';  
}  
int main()  
{  
    char c[10] = "Hello";  
    change(c);  
    puts(c);  
    return 0;  
}
//输出：
//HELlo
~~~

## 6.4 指针与malloc动态内存申请，栈与堆的差异

### 6.4.1 指针与动态内存申请

之前定义的各种类型的变量都是在栈空间的，而栈空间的大小在编译时就是确定的。如果使用的空间的大小是不确定的，那么就要使用堆空间。

~~~c
#include <stdlib.h>
void *malloc(size_tsize);//申请空间
.....
free(size_tsize)//释放空间
~~~

默认返回得是无类型指针`void *`，无法进行偏移，所以需要强制转换类型

~~~c
#include <stdio.h>  
#include <stdlib.h>//malloc使用的头文件  
#include <string.h>  
int main()  
{  
    int size ;//代表要申请多大的空间  
    char *p;//void*类型的指针不能进行偏移操作
    scanf("%d",&size);//输入想要申请的空间大小  
    p=(char*)malloc(size);//申请size大小的空间  
    strcpy(p,"Hello World");  
    puts(p);  
    free(p);//释放申请的空间  
    return 0;  
}
~~~

* 添加malloc使用的头文件`#include <stdlib.h>`
* 申请空间太小会存在堆空间的访问越界
* 强制转换返回类型
* 最后**释放**申请的空间（**非常重要**），给的地址必须时最初malloc返回的地址
* 注意指针本身的大小和其指向空间的大小是两码事，不能和前面的变量类比去理解

### 6.4.2 栈空间与堆空间

**栈 stack**是计算机系统提供的数据结构，计算机会在底层对栈提供支持：分配专门的寄存器存放栈的地址，压栈操作、出栈操作都有专门的指令执行，这就决定了栈的效率比较高。

**堆 heep**则是 C，C++函数库提供的数据结构，它的机制很复杂，例如为了分配一块内存，库函数会按照一定的算法（具体的算法请参考关于数据结构、操作系统的书籍)在堆内存中搜索可用的足够大小的空间，如果没有足够大小的空间(可能由于内存碎片太多），那么就有可能调用系统功能去增加程序数据段的内存空间，这样就有机会分到足够大小的内存，然后返回。

**堆的效率要比栈低得多。** 栈空间由系统自动管理，而堆空间的申请和释放需要自行管理（c语言中的malloc函数为例），所以在具体例子中需要通过 free 函数释放堆空间。

~~~c
#include <stdio.h>  
#include <stdlib.h>  
#include <string.h>  
  
char* print_stack()  
{  
    char c[100]="I'm a print_stack function";  
    char *p;  
    p=c;  
    return p;  
}  
  
char* print_malloc()  
{  
    char *p = (char *)malloc(100);//堆空间在整个运行中一直有效，不会因为函数的消亡而结束。  
    strcpy(p, "I'm a print_malloc function");  
    return p;  
}  
  
int main()  
{  
    char *p;  
    p = print_stack();  
    puts(p);  
    p = print_malloc();  
    puts(p);  
    free(p);// 释放堆空间  
    return 0;  
}
//结果：
//乱码
//I'm a print_malloc function
~~~

* 堆空间在整个运行中一直有效，不会因为函数的消亡而结束。
* 只有free时堆空间才会释放


# 7. 函数

## 7.1 函数的声明与定义-嵌套调用

### 7.1.1 函数的声明与定义

函数间的调用关系是，由主函数（main函数）调用其他函数，其他函数也可以互相调用，但是其他函数不可以调用main函数。同一个函数可以被一个或多个函数调用任意次。

~~~c
#include <stdio.h>  
void print_message();//声明函数  
int main()  //主函数
{  
    print_message();//调用函数  
    return 0;  
}  
void print_message()//定义函数  
{  
    printf("How do you do\n");  
}
~~~

* 声明函数
* 调用函数（在main函数或者其他函数中）
* 定义函数

~~~c
//func.h

#ifndef FUNC_H  
#define FUNC_H  
//放在这里避免重复编写  
#include <stdio.h>  
//函数声明，可以调换顺序  
void print_message();  
int print_star(int i);  

#endif //FUNC_H
~~~

~~~c
//main.c

#include "func.h"  
//函数的声明写到了func.h文件中，头文件也写到了func.h文件中 
int main()  
{  
    int a = 10;  
    a = print_star(a);//调用函数，接收返回值  
    print_message();//调用函数  
    print_star(5);  
    return 0;  
}  
//函数的定义放到了func.c文件中
~~~

~~~c
//func.c

#include "func.h"  
int print_star(int i)  
{  
    printf("---------------\n");  
    printf("print_star %d\n",i);  
    return i+3;  
}  
void  print_message()//可以嵌套调用上面的函数  
{  
    printf("How do you do\n");  
    print_star(6);//这里的返回值可以选择接收或者不接收  
}
~~~

1. 一个C程序由一个或多个程序模块组成，每个程序模块作为一个源程序文件。对于较大的程序，通常将程序内容分别放在若干源文件中，再由若干源程序文件组成一个C程序。这样处理便于**分别编写、分别编译，进而提高调试效率(复试有用)**。一个源程序文件可以为多个C 程序共用。
2. 一个源程序文件由一个或多个函数及其他有关内容(如命令行、数据定义等)组成。一个源程序文件是一个编译单位,在程序编译时是以源程序文件为单位而不是以函数为单位进行编译的。main.c和 func.c分别单独编译，在链接成为可执行文件时，main 中调用的函数print_star 和print_ message 才会通过链接去找到函数定义的位置。
3. C程序的执行是从 main 函数开始的，如果在 main 函数中调用其他函数，那么在调用后会返回到 main 函数中，在 main 函数中结束整个程序的运行。
4. 所有函数都是平行的，即在定义函数时是分别进行的，并且是互相独立的。一个函数并不从属于另一函数,即函数不能嵌套定义。函数间可以互相调用，但不能调用 main 函数。

函数的定义和声明的区别：

* 函数的定义是指对函数功能的确立，包括指定函数名、函数值类型、形参及其类型、函数体等，它是一个完整的、独立的函数单位。
* 函数的声明的作用是把函数的名字、函数类型及形参的类型、个数和顺序通知编译系统，以便在调用该函数时编译系统能正确识别函数并检查调用是否合法。

**不要使用隐式声明**，C语言中有几种声明的类型名可以省略，称之为隐式声明

### 7.1.2 函数的分类与调用

* 标准函数：即库函数，这是由系统提供的，用户不必自己定义的函数，可以直接使用，如 printf 函数、scanf 函数。不同的C系统提供的库函数的数量和功能会有一些不同，但许多基本的函数是相同的。
* 用户自己定义的函数：用以解决用户的专门需要

* 无参函数:一般用来执行指定的一组操作。在调用无参函数时，主调函数不向被调用函数传递数据。
* 有参函数：主调函数在调用被调用函数时，通过参数向被调用函数传递数据。


## 7.2 函数的递归调用

* 函数调用自身的操作称之为递归
* 递归函数一定要有结束条件（写在return之前），否则会产生死循环。
* 递归的核心就是**找公式**

~~~c
//求函数的阶乘
#include <stdio.h>  
int f(int n)  
{  
    if (n == 1) //设置结束条件
    {  
        return ;  
    }  
    return n*f(n-1);//找到的公式f(n)=f(n)*f(n-1)
}  
int main()  
{  
    int i ;  
    scanf("%d",&i);  
    printf("%d",f(i));  
    return 0;
}
~~~




## 7.3 局部变量与全局变量

### 7.3.1 全局变量-形参-实参

不同函数之间传递数据时：

* 参数：通过形式参数和实际参数
* 返回值：用return语句返回结果
* 全局变量：外部变量

~~~c
//全局变量演示（尽量不要使用）
#include <stdio.h>  
int i = 10;//i是一个全局变量  
void print(int a)  
{  
    printf("print i=%d\n", i);  //访问的全局变量i
}  
int main()  
{  
    printf("main i=%d\n", i);  //访问的全局变量i
    i = 5;  //修改全局变量i的值
    int i = 10;//创建了一个局部变量i
    print(5);  
    return 0;  
}
//输出
//main i=10
//print i=5 输出的是数据段中的全局变量i
~~~

* 在这里的全局变量i既不存在于栈空间，也不存在于堆空间，它存在于数据段
* 如果局部变量与全局变量重名，那么将采取**就近原则**
* 尽量不要使用全局变量

形式参数与实际参数

1. 定义函数中指定的形参，如果没有函数调用，那么它们并不占用内存中的存储单元。只有在发生函数调用时，函数 print 中的形参才被分配内存单元。在调用结束后，形参所占的内存单元也会被释放。
2. 实参可以是常量、变量或表达式，但要求它们有确定的值，例如，print(i+3)在调用时将实参的值 i+3 赋给形参；print 函数可以有两个形参，如print(int a,int b)
3. 在被定义的函数中，必须指定形参的类型。如果实参列表中包含多个实参，那么各参数间用逗号隔开。**实参与形参的个数应相等**，类型应匹配，且实参与形参应按顺序对应，一一传递数据。
4. 实参与形参的类型应相同或赋值应兼容。
5. 实参向形参的数据传递是单向“值传递”，只能由实参传给形参，而不能由形参传回给实参。在调用函数时，给形参分配存储单元，并将实参对应的值传递给形参，调用结束后，形参单元被释放，实参单元仍保留并维持原值。
6. 形参相当于局部变量，因此不能再定义局部变量与形参同名，否则会造成编译不通。

### 7.3.2 局部变量与全局变量

内部变量（局部变量）

1. 主函数中定义的变量只在主函数中有效，而不因为在主函数中定义而在整个文件或程序中有效。主函数也不能使用其他函数中定义的变量。
2. 不同函数中可以使用相同名字的变量，它们代表不同的对象，互不干扰。
3. 形式参数也是局部变量。
4. 在一个函数内部，可以在复合语句中定义变量，这些变量只在本复合语句中有效，这种复合语句也称“分程序”或“程序块”。
5. 注意一个细节，for 循环的小括号内定义的int，在离开 for 循环后，是不可以再次使用。

外部变量（全程变量，全局变量）

1. 全局变量在程序的全部执行过程中都占用存储单元，而不是仅在需要时才开辟单元。
2. 使用全局变量过多会降低程序的清晰性，在各个函数执行时都可能改变外部变量的值,程序容易出错(初试时尽量不用)。
3. 因为函数在执行时依赖于其所在的外部变量，如果将一个函数移到另一个文件中，那么还要将有关的外部变量及其值一起移过去。然而，如果该外部变量与其他文件的变量同名，那么就会出现问题，即会降低程序的可靠性和通用性。**C语言一般要求把程序中的函数做成一个封闭体，除可以通过“实参一形参”的渠道与外界发生联系外，没有其他渠道**。

# 8. 结构体与C++引用
## 8.1 结构体-结构体对齐-结构体数组

###  8.1.1 结构体的定义、初始化、结构体数组

C语言提供结构体来管理不同类型的数据组合

~~~c
struct 结构体名
{
	成员表列
};
~~~

~~~c
#include <stdio.h>  
struct student  
{  
    int num;char name[20];  
    char sex;  
    int age;  
    float score;  
    char addr[30];  
};  
int main()  
{  
    struct student st ={1001,"tim",'M',20,99,"chengdu"};//定义结构体变量  
    scanf("%d%s %c%d%f%s",&st.num,st.name,&st.sex,&st.age,&st.score,st.addr);
    printf("%s\n",st.name);
}
~~~

* 结构体类型声明，注意最后一定要加分号 
* addr和name里面不需要加&，因为数组名本身就包含了指针
* 结构体输出必须某单独去访问内部的单个成员`&starr[i]`

~~~c
#include <stdio.h>  
  
struct student  
{  
    int num;char name[20];  
    char sex;  
    int age;  
    float score;  
    char addr[30];  
};  
//结构体类型声明，注意最后一定要加分号  
  
int main()  
{  
    struct student starr[3];  
    int i;  
    for(i=0;i<3;i++)  
    {  
        scanf("%d%s %c%d%f%s",&starr[i].num,starr[i].name,&starr[i].sex,&starr[i].age,&starr[i].score,starr[i].addr);  
    }  
    for(i=0;i<3;i++)  
    {  
        printf("%s\n",starr[i].name);  
    }  
}
~~~

* 结构体数组和数组类似的操作方式

### 8.1.2 结构体对齐（计算结构体的大小）

* 结构体的大小必须是其组大成员的整数倍。（数据类型的最大成员，不是包含数组）
* 对齐的目的是为了让cpu高效的去读取内存中的数据。（方便数据总线去读取）

~~~c
#include <stdio.h>  
struct student_type1  
{  
    double score;  
    short age;  //8+2 <16 = 8*2
};  
struct student_type2  
{  
    double score;  
    int height;  
    short age;  //8+4+2 <16 = 8*2
};  
struct student_type3  
{  
    int height;  
    char sex;  
    short age;  //4+1+2 < 8 = 5*2
};  
int main()  
{  
    struct student_type1 s1;  
    struct student_type2 s2;  
    struct student_type3 s3;  
    printf("%d %d %d\n", sizeof(s1), sizeof(s2), sizeof(s3));  
    return 0;  
}
//结果
//16 16 8
~~~

* 在这里的`student_type2`中，`height`和`age`会共同占8个字节；在`student_type3`中的`sex`和`age`也会是这样的。
* 如果2个小存储之和是小于最大长度就会放在一起。

## 8.2 结构体指针与typedef的使用

### 8.2.1 结构体指针

一个结构体变量的指针就是该变量所占据的内存段的起始地址。

~~~c
#include <stdio.h>  
struct student  
{  
    int num;  
    char name[20];  
    short sex;  
};  
int main()  
{  
    struct student s = {1001,"leile",'M'};  
    struct student sarr[3] = {1002,"wangle",'M',1003,"lie",'F',1004,"Sharvime",'M'};  
    struct student *p;//定义结构体指针变量  
    int num;  
    p=&s;  
    printf("%d %s %c\n",p->num,p->name,p->sex);//考试就这么写，  
    printf("%d %s %c\n",(*p).num,(*p).name,(*p).sex);//几乎不用这种  
    p = sarr;  
    printf("%d %s %c\n",p->num,p->name,p->sex);  
    p=p+1;  
    printf("%d %s %c\n",p->num,p->name,p->sex);  
    return 0;  
}
~~~


### 8.2.2 typedef的使用

typedef是给取别名，可以给结构体和结构体指针等起别名，方便使用。

~~~c
#include <stdio.h>  
typedef struct student  
{  
    int num;  
    char name[20];  
    short sex;  
}stu,*pstu;  
typedef int INGETER;//特定地方使用  
int main()  
{  
    stu s = {1001,"wangle",'M'};  
    stu *p = &s;//定义了一个结构体指针变量，等价struct student *p  
    pstu p1 = &s;//定义了一个结构体指针变量  
    INGETER num;  
    printf("i=%d,p->num=%d\n",num,p->num);  
    return 0;  
}
~~~

* `stu`等价于`struct student`
* `pstu`等价于`struct student *`

## 8.3 C++的引用

### 8.3.1 C++的引用


~~~cpp

#include <stdio.h>  
void modify_num(int &b)  
{  
    b=b+1;  
}  
  
int main()  
{  
    int a = 10;  
    modify_num(a);  
    printf("after modify_num=%d\n",a);  
    return 0;  
}
//结果：
//after modify_num=11
~~~

* 添加引用的作用，在子函数中修改主函数的普通变量
* 形参中写`&`，要称为引用
* 引用必须和变量名紧邻
* 在纯C语言程序中想要这样的结果需要使用指针变量来实现

~~~cpp
#include <stdio.h>  
void modify_pointer(int *&p,int *q)  
{  
    p=q;  
} 
int main()  
{  
    int *p=NULL;  
    int i =10;  
    int *q=&i;  
    modify_pointer(p,q);  
    printf("after modify_pointer *p=%d",*p);  
    return 0;  
}
~~~

* 添加引用的作用，在子函数中修改主函数的指针变量
* 纯c语言实现需要使用二级指针（无需掌握）


### 8.3.2 C++的布尔类型

布尔类型在 C语言没有，但是 C++有，有 true 和 false

~~~cpp
#include <stdio.h>  
int main()  
{  
    bool a = true;  
    bool b = false;  
    printf("a=%d b=%d\n", a,b);  
    return 0;  
}
//结果
//a=1 b=0
~~~
