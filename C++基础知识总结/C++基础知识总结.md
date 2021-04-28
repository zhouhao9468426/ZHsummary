# C++关键字  
>https://blog.csdn.net/qq_35671135/article/details/88092382  
### 1.关键字分类及简介   
>https://www.runoob.com/w3cnote/cpp-keyword-intro.html  
>https://blog.csdn.net/qq_35671135/article/details/88092382
>https://www.cnblogs.com/zxj9487/p/10964968.html

数据类型：void，int，char，float，double，bool，w_char  
类型定义：struct，union，enum，class，typedef  
常量值：true，false  
类型修饰符：long，short，singed，unsigned  
类型限定符：const，volatile，restrict  
存储说明符：auto，register，static，extern，thread_local，mutable  
其它修饰符：inline，asm  
循环控制：for，while，do  
跳转控制：break，continue，return，goto  
分支结构： if，else，switch，case，default  
内存管理：new, delete  
运算符：sizeof，and，and_eq，bitand，bitor，compl，not，not_eq，or，or_eq，xor，xor_eq  
访问限定符：this，friend，virtual，mutable，explicit，operator  
类访问修饰符：private，protected，public  
模板：template，typename  
命名空间：namespace，using  
异常处理：throw，try，catch  
***  
### 1.1数据类型相关  
>https://www.runoob.com/cplusplus/cpp-data-types.html  
>https://blog.csdn.net/qq_35671135/article/details/88092382
***  
(1)bool、true、false  
bool是布尔类型，属于基本类型中的整数类型，取值分为true和false。true和false是具有布尔类型的字面量，是右值（https://zhuanlan.zhihu.com/p/240833006）  
tips:  
1)左值可在等号左右两边，右值只能在等号右边；  
2)左值可以寻址，有持久性；  
3)右值一般是表达式求值过程中创建的无名临时对象，短暂性的。  
4)左值可以被修改而右值不能  
5)左值引用：引用一个对象  
6)右值引用：必须绑定到右值的引用，C++11中右值引用可以实现“移动语义”  
7)右值引用和相关的移动语义可以避免无谓的复制，提高程序性能  
>https://www.jianshu.com/p/94b0221f64a5  
```
int x = 6;
int &y = x;
int &z1 = x*6; //不能将一个非常量左值引用与一个右值绑定
const int &z2 = x * 6; //常量左值引用可以与右值绑定
int &&z3 = x * 5;      //正确的右值引用
cout << z2 << endl;
int &&z4 = x; //不能将一个右值引用与一个左值绑定
```   
***
(2)char、wchar_t  
char表示单字节字符，wchar_t表示多字节字符，char、signed char、unsigned char表示了有符号字符和无符号字符两种类型，char不同编译器可能不同；宽字符的定义是为了可以简化使用国际通用字符集进行的编程(typedef unsigned short wchar_t)  
*** 
(3) int、double、float、short、long、signed、unsigned    
signed和unsigned作为前缀修饰整数类型，分别表示有符号和无符号。signed和unsigned修饰char类型，构成unsigned char和signed char，和char都不是相同的类型；不可修饰wchar_t、char16_t和char32_t。其它整数类型的signed省略或不省略，含义不变。signed或unsigned可单独作为类型，相当于signed int和unsigned int。
double和float专用于浮点数，double表示双精度，精度不小于float表示的浮点数。long double则是C++11指定的精度不小于double的浮点数。   
***
(4)explicit
explicit在C++中一般只用来修饰构造函数，可以禁止编译器自动调用拷贝初始化(对应直接初始化)，还可以禁止编译器对拷贝函数的参数进行隐式转换
1)explicit只能在类内部使用，不能在外面声明  
2)声明了explicit之后，就不允许进行隐式转换了
3)explicit对于多个参数的构造函数没有任何意义  
```
#include<iostream>
using namespace std;
class test
{
public:
    explicit test(int x)
    {
        cout << "the int is called" << endl;
    }
    test(char ch)
    {
        cout << "the char is called" << endl;
    }
    test()
    {
        cout << "the none is called" << endl;
    }
    test(int x, int y)
    {
        cout << "the two is called" << endl;
    }
    test(const test& a)
    {
        cout << "the copy is called" << endl;
    }
};
int main()
{
    test A = 10;
    test A1 = 'c';
    return 0;
}
```   
***  
(5)auto decltype
1)auto  
将表达式的值赋给变量，需要清楚地知道表达式的类型，auto关键字会根据初始值自动推断变量的数据类型,不是每个编译器都支持auto。 
auto x = 7;//推断出x类型为int  
auto i=0,j=3.14;//出错，一条声明语句只能有一个数据类型  
2)decltype  
希望从表达式的类型推断出要定义的变量的类型，但是不想用该表达式的值初始化变量，此时可以使用decltype  
decltype(i+j) x = 0;  
```
#include<iostream>
using namespace std;
int main()
{
    int x = 3;
    double y = 3.14;
    cout << x+y << endl;
    decltype(x+y) res = x+y;
    cout << res << endl;
    return 0;
}
```  
### 1.2 定义、初始化相关  
***  
(1)const、volatile、mutabale、const_cast
1)const  
const 是 constant 的缩写，本意是不变的，不易改变的意思。在 C++ 中是用来修饰内置类型变量，自定义对象，成员函数，返回值，函数参数。C++ const 允许指定一个语义约束，编译器会强制实施这个约束，允许程序员告诉编译器某值是保持不变的。如果在编程中确实有某个值保持不变，就应该明确使用const，这样可以获得编译器的帮助。
2)volatile   
- 当读取一个变量时，为提高存取速度，编译器优化时有时会先把变量读取到一个寄存器中，以后再取变量值时，就直接从寄存器中取值
- 优化器在用到volatile变量时必须每次都小心地重新读取这个变量的值，而不是使用保存到寄存器里的备份。
- volatile适用于多线程应用中被几个任务共享的变量。
#### 一、const修饰普通类型变量 
(1)不能对一个常量赋值
```
const int a = 7;
int b = a; //可以用一个常量给变量赋值
a = 8;	//错误
```
尝试修改一个const变量，调试窗口中可以看到a的值已经变成了8，但输出结果仍然是7.
```
#include<iostream>
using namespace std;
int main()
{
    const int a = 7;
    int *p = (int*)&a; 
    *p = 8;
    cout << a << endl;
    return 0;
}
```
在const之前加上volatile就能成功改变  
```
#include<iostream>
using namespace std;
int main()
{
    volatile const int a = 7;
    int *p = (int*)&a; 
    *p = 8;
    cout << a << endl;
    return 0;
}
```    
#### 二、const修饰指针变量  
(1)const修饰指针所指的对象，对象不可改变   
const int* p = 8;
(2)const修饰指针本身，指针本身不能变,内容可变    
int *const int  
(3)两者都修饰  
const int *const int   
***  
#### 三、const参数传递和函数返回值   
修饰函数参数有三种情况：
(1)值传递的const修饰，*一般而言这种情况不需要const修饰，因为函数会自动产生临时变量复制实参*  
```
#include<iostream>
using namespace std;
void print(int a)
{
    a++;
    cout << a << endl;
}
int main()
{
    int num = 1;
    print(num);
    cout << num << endl;
    return 0;
}
```  
(2)指针传递的const修饰，可以防止指针被恶意篡改  
```
#include<iostream>
using namespace std;
void print(int *const a)
{
    cout << *a << endl;
    *a = 9;
    //int b = 5;
    //a = &b;//常量指针不能修改
}
int main()
{
    int num = 1;
    print(&num);
    cout << num << endl;
    return 0;
}
```  
(3)自定义类型的参数传递，需要临时对象复制参数，对于临时对象的构造，需要调用构造函数，比较浪费时间，因此采用const加引用传递的方式，对于内置类型一般不需要采用引用传递的方式。
***  
const修饰函数返回值  
(1)内置类型返回值不需要const修饰，即使加了也没效果  
```
#include<iostream>
using namespace std;
const int test()
{
    return 1;
}
int main()
{
    int m = test();
    cout << m << endl;
    m++;
    cout << m << endl;
    return 0;
}
```   
(2)const 修饰自定义类型的作为返回值，此时返回的值不能作为左值使用，既不能被赋值，也不能被修改。  
(3)const 修饰返回的指针或者引用，是否返回一个指向 const 的指针，取决于我们想让用户干什么。  
```
#include<iostream>
using namespace std;
int a = 1;
const int* test()
{
    return &a;
}
int main()
{
    const int* p = test();
    //(*p)++;//出错，不能修改
    a++;//输出2
    cout << *p << endl;
    return 0;
}
```  
*** 
#### 四、const修饰类成员函数  
const 修饰类成员函数，其目的是防止成员函数修改被调用对象的值，如果我们不想修改一个调用对象的值，所有的成员函数都应当声明为const成员函数。
注意：const 关键字不能与 static 关键字同时使用，因为 static 关键字修饰静态成员函数，静态成员函数不含有 this 指针，即不能实例化，const 成员函数必须具体到某一实例。  
```
#include<iostream>
using namespace std;
class Test
{
    int a;
    mutable int b;
public:
    Test(int x, int y):a(x),b(y){}
    void test() const
    {
        //a++; //错误
        b++;
    }
};
int main()
{
    Test test(8,7);
    return 0;
}
```  
#### 五、const相关知识点  
(1)const数据成员只在某个对象的生存期内是常量，对于整个类而言是可变的。因为类可以创建多个对象，不同的对象其const数据成员的值可以不同。所以不能在类的声明中初始化const数据成员，因为类的对象没被创建时，编译器不知道const数据成员的值是什么。const数据成员的初始化只能在类的构造函数的初始化列表中进行。要想建立在整个类中都恒定的常量，应该用类中的枚举常量来实现，或者static const。
(2)C++11之后允许const成员变量直接初始化
(3)非常量对象可以调用常量成员函数与非常量成员函数，若量函数重载，非常量对象自动调用非常量成员函数版本  
(4)常量对象只能调用常量成员函数，不能调用非常量成员函数  
```
#include<iostream>
using namespace std;
class Test
{
    const int a=5;//C++11之后支持
    int b;
public:
    Test(int x, int y):a(),b(y){} //初始化列表初始const数据成员
    void test() const
    {
        cout << "const_fun is called" << endl;
        cout << a << ' ' << b << endl;
    }
    void test()
    {
        //b++; //非常量成员函数可以修改成员变量
        cout << "fun is called" << endl;
        cout << a << ' ' << b << endl;
    }
};
int main()
{
    Test test(8,7);
    test.test();//优先调用nonconst函数
    const Test test1(8,7);
    test1.test();
    return 0;
}
```  
(5)const成员函数，不能调用非const成员函数，不能修改成员变量 
(6)const成员变量可以通过初始化列表或者类外初始化(C++11之前)
(7)const对象默认为文件局部变量，想在其他文件中访问const变量，必须在文件中显示的指出extern  
(8)const 与 define  
>https://light-city.club/sc/basic_content/const/  
- 定义常量：const int a = 100;
- 类型检查：const定义常量可以进行类型检查
- 节省空间：从汇编角度看，const定义的常量给出了地址，而define给出的是立即数，const定义的常量在程序运行过程中只有一份拷贝，而define可能有多份
- const定义的变量只有类型为整数或者枚举，且以常量表达式初始化才能作为常量表达式。其他情况下只是const限定的变量。  
(9)提高效率：编译器通常不为普通const常量分配存储空间，而是将他们保存在符号表中，这使得它成为一个编译期间的常量，没有了存储与读内存的操作。第一次使用时为其分配内存，在程序结束的时候释放；const全局变量存储在只读数据段中，const局部变量存储在栈中。  
(10)const_cast(expression)可修改const属性。  
(11)整个类的常量的实现(希望某些常量只在类中有效)  
```
方法一：
class A
{
    enum{SIZE1 = 100, SIZE2 = 200};        //枚举常量 
    int arrayA[SIZE1];
    int arrayB[SIZE2];
};
方法二：
class Test  
{  
public:  
      Test():a(0){}  
      enum {size1=100,size2=200};  
private:  
      const int a;//只能在构造函数初始化列表中初始化  
       static int b;//在类的实现文件中定义并初始化  
      const static int c;//与 static const int c;相同。  
};  
  
int Test::b=0;//static成员变量不能在构造函数初始化列表中初始化，因为它不属于某个对象。  
cosnt int Test::c=0;//注意：给静态成员变量赋值时，不需要加static修饰符。但要加cosnt  
```   
(12)const变量必须进行初始化  
(13)一点有意思的语法  
```
#include<iostream>
using namespace std;
int main()
{
    double dval = 3.14;
    const int &ri = dval; //int &ri = dval;错误
    cout << ri << endl;
    return 0;
}
```   
(14)常量表达式是指值不会改变并且在编译过程中就能得到计算结果的表达式
***
##### (2)enum关键字的应用
>https://www.runoob.com/w3cnote/cpp-enum-intro.html  
enum 类型名 {枚举常量表}；   
enum fruit_set {apple, orange, banana=1, peach, grape}//枚举常量apple=0,orange=1, banana=1,peach=2,grape=3。  
tips:  
(1)枚举常量不会占用对象空间，他们在编译时被全部求值  
(2)隐含的数据类型是整数，最大值有限而且不能表示浮点数
***
##### (3)export  
使用该关键字可实现模板函数的外部调用。对模板类型，可以在头文件中声明模板类和模板函数；在代码文件中，使用关键字export来定义具体的模板类对象和模板函数；然后在其他用户代码文件中，包含声明头文件后，就可以使用该这些对象和函数  
***
##### (4)extern  
1)extern介绍
extern（外部的）声明变量或函数为外部链接，即该变量或函数名在其它文件中可见。被其修饰的变量（外部变量）是静态分配空间的，即程序开始时分配，结束时释放。用其声明的变量或函数应该在别的文件或同一文件的其它地方定义（实现）。在文件内声明一个变量或函数默认为可被外部使用。在 C++ 中，还可用来指定使用另一语言进行链接，这时需要与特定的转换符一起使用。目前仅支持 C 转换标记，来支持 C 编译器链接。使用这种情况有两种形式：
extern "C"
{
}
extern const改变const变量可见性  
2)C++与C编译的区别  
在C++中常在头文件见到extern "C"修饰函数，那有什么作用呢？ 是用于C++链接在C语言模块中定义的函数。

C++虽然兼容C，但C++文件中函数编译后生成的符号与C语言生成的不同。因为C++支持函数重载，C++函数编译后生成的符号带有函数参数类型的信息，而C则没有。

例如int add(int a, int b)函数经过C++编译器生成.o文件后，add会变成形如add_int_int之类的, 而C的话则会是形如_add, 就是说：相同的函数，在C和C++中，编译后生成的符号不同。

这就导致一个问题：如果C++中使用C语言实现的函数，在编译链接的时候，会出错，提示找不到对应的符号。此时extern "C"就起作用了：告诉链接器去寻找_add这类的C语言符号，而不是经过C++修饰的符号。  
3)C++和C互调方法  
```
C++调用C
//xx.h
extern int add(...)

//xx.c
int add(){

}

//xx.cpp
extern "C" {
    #include "xx.h"
}
C调用C++
//xx.h
extern "C"{
    int add();
}
//xx.cpp
int add(){

}
//xx.c
extern int add();
```
##### (5)public、protected、private  
1)这三个都为权限修饰符。public为公有的，访问不受限制；protected为保护的，只能在本类和友元中访问；private为私有的，只能在本类、派生类和友元中访问。
2)class默认访问权限是private的，struct默认访问权限是public的。  
3)public继承，派生类成员函数可以访问public、protected成员变量，派生类实例只能访问public成员变量；protected继承，派生类成员函数可以访问public、protected成员变量，派生类实例无法访问任何基类成员；private继承，成员函数可以访问public、protected成员，派生类实例无法访问任何基类成员。  
##### (6)template  
声明一个模板，模板函数，模板类等。模板的特化。  
##### (7)static  





   


