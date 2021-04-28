# C++关键字  
>https://blog.csdn.net/qq_35671135/article/details/88092382  
###1.关键字分类及简介   
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
###1.1数据类型相关  
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
(1)const、volatile  
1)const  
const 是 constant 的缩写，本意是不变的，不易改变的意思。在 C++ 中是用来修饰内置类型变量，自定义对象，成员函数，返回值，函数参数。C++ const 允许指定一个语义约束，编译器会强制实施这个约束，允许程序员告诉编译器某值是保持不变的。如果在编程中确实有某个值保持不变，就应该明确使用const，这样可以获得编译器的帮助。





   


