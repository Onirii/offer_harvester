# 1. 模板概念，函数模板定义与调用
## 1.1 概述
1. 所谓泛型编程，是以独立于任何特定类型的方式编写代码。使用泛型编程时，需要提供程序实例所操作的类习惯或者值。
2. 模板是泛型编程的基础。模板是创建类或者函数的蓝图或者公式。给模板提供足够的信息，就能让这些蓝图或者公式生成真正的类或者函数。
3. 模板是支持将类型作为参数的程序设计方式，从而实现了对泛型程序设计的直接支持。也就是说，C++模板机制允许在定义类、函数时将类型作为参数。
4. 模板一般分为函数模板和类模板。

## 1.2 函数模板的定义
1. 模板定义以template关键字开头，后跟<>，<>中为模板参数列表，各参数间以","分隔开。每个模板参数前有template/class关键字。
2. 模板参数列表里边表示在函数定义中用到的“类型”或者“值”，和函数参数列表类似。
3. 编译器会在编译时候根据对模板函数的调用来确定模板参数的具体类型。

## 1.3 函数模板的使用
1. 函数模板调用和函数调用差别不大，在函数模板调用时，编译器会根据被调用的函数模板的实参去**推断**模板参数列表里的参数的具体类型。当编译器凭借函数实参无法推断模板参数时，就需要用<>来主动提供模板参数。
2. 编译器在推断出模板函数的形参类型之后，将实例化一个特定版本的函数。

## 1.4 非类型模板参数
1. 非类型参数代表一个值，