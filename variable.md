# 变量与常量

变量与常量是一切编程语言的基础，因此开篇来讨论一下变量与常量。

## 类型
在接触变量之前，我们先来定义一个概念：类型。

如果cpp是你接触的第一门语言，那么你只要记住一件事：任何数据都是有类型的；大多数类型都是不能够直接转换的。 

比如，`10`的类型是数字；`"abc"`的类型是字符串；`true`就是布尔型。

在cpp中，通常我们用`int`来表示整数，用`double`来表示小数，用`string`来表示字符串，等等。

后续我们还会讲到类型

## 变量

Cpp是静态类型语言，也即在声明任何变量之前，需要给出它的类型。比如：

```cpp
int a = 10;
```

在上例中，`int` 是类型，`a` 是变量名，`10` 则是变量被赋的初值。

> 在Cpp中，变量一旦声明，它的类型即不可改变。

> 在Cpp中，若变量没有被赋与初值，则它可能会被初始化为任意值，容易导致代码中出现隐形bug，所以建议在声明任何变量时即给它赋一个初值。

声明了变量`a` 之后，在它的 \*生命周期\* 内，我们可以用`a` 来获得或改变它的值。比如：

```cpp
int b = a + 4; // 我们声明了变量b，类型是 int，初值是 14 (10 + 4)
a = a + 2;     // a被重赋值为12 (10 + 2)
int c = a + b; // 声明变量c，类型是int，初值是 26 (12 + 14)
```

在上例中，我们利用变量`a` 的值10，计算出`a + 4` 为14并赋值给新声明的变量`b` 。接着我们将`a` 的值从10 改变成12。这里注意，在我们计算`a = a + 2` 时，我们先对右侧的\*表达式\*求值，这时`a`的值为10，所以`a + 2` 的结果为12。然后我们再将得到的新值12赋给左侧的变量`a`。最后，我们再计算`a + b` 并将结果26赋给新的变量c。

## 常量

常量与变量在逻辑上的区别就是，变量在声明后可以随意改变值；而常量一旦赋值后即不可变，否则编译会发生错误。另一点小区别就是，常量必须在声明时定义值，而不能先声明，再定义。

在Cpp中，常量分为两种，一种是编译期常量，即在编译时就知道其值的常量；一种是运行时常量，只有在运行时才知道其值的常量。

### 编译期常量

编译期常量用`constexpr` 来修饰。比如：

```cpp
constexpr int kConstant = 10;  // kConstant 为int类型的编译期常量，它的值为10
```

与变量不同，编译器常量需在声明时立刻赋值，随后在它的整个声明周期中，其值均不可变。编译期常量正如其名，其值是可以在编译期推断出来。因而编译器常量的值可以从其他编译期常量计算出来，但不能从动态常量中计算出来，因运行时常量是在运行时推断出来的。

```cpp
constexpr int kAnotherConst =  kConstant + 11; // kAnotherConst 为 21(10 + 11) 
```



### 运行时常量

与编译期常量不同，尽管同为常量，但是运行时常量的值必须在运行时才能确定：比如用户输入，网络传输数据，数据库读取的数据等。

cpp用`const` 关键字来修饰运行时常量。比如：

```cpp
const int p = get_runtime_input(); // 获取 runtime_input，并赋给常量 p
```

`p` 在使用时与编译期常量类似，一旦赋值后即不可改变。

### 常量的意义

很多初学者第一次接触常量时可能觉得这个概念很无厘头：既然已经有变量了，为什么还要有有常量呢？

这其实是因为在Cpp中，有许多计算是在编译时进行的：比如模板推导，数组空间分配等。因此编译期常量在这些场景下很有作用。

另一个原因则是，在Cpp中，我们通常用`const`来修饰传递变量，来标明此变量在当下作用域不可变。

比如：
```cpp
int max1(int& a, int& b){
// 逻辑...
}

int max2(const int& a, const int& b){
// 逻辑...
}
```
在上例中，`max1`接受的两个参数均是变量，调用者不确定`a`与`b`的值是否会改变。而`max2`标明接受的两个参数`a`与`b`在当前作用域为常量，调用者很明确参数不会被修改。

## 生命周期与作用域

在讲解变量与常量时，我们提到了*作用域*与*生命周期*这两个概念。接下来详细的讲解一下这两个概念分别是什么意思。

### 生命周期

顾名思义，生命周期就是一个变量或常量的有效期。通常，变量或常量并不会在整个代码内一直有效。比如，小王和小张合作编写项目A。小王在他的代码里他声明了一个变量`a`，而小李在它的代码里也声明了变量`a`。那编译器如何判断在小王的代码里该用哪个`a`，在小张的代码里该用哪个`a`呢？ 要讨论这个问题，我们需要引入作用域。

### 作用域

作用域指的是变量理论上有效的源代码区域。听起来很抽象对不对？简单来说，一个以`{`开始，以`}`结束的代码块就是一个作用域。作用域可以互相嵌套。在变量/常量定义的作用域极其子作用域内，变量/常量会一直有效；而超出作用域后，该变量即不可用。所以，**变量的声明周期是从它被声明开始，到它对应的作用域结束为止**。比如：

```cpp
{                           // 作用域1 开始
    int a = 10;             // 定义变量a
    std::cout << a;         // 输出变量a的值，10

    {                       // 子作用域 开始
        std::cout << a;     // 输出10，引用到父作用域的 a
        int a = 11;         // 子作用域内重定义 a，覆盖父作用域的a

        std::cout << a;     // 输出11，引用到父作用域的 a
        int b = 20;         // 子作用域内重定义 a，覆盖父作用域的a
        std::cout << b;     // 输出20，引用到父作用域的 a
    }                       // 子作用域 结束. 子作用域的变量a与b生命周期结束。

    std::cout << b          // 错误，在作用域1 中没有变量声明变量b
}                           // 作用域1 结束，a的生命周期结束。
```
> 注： 在同一作用域中，不能声明同名变量。

> 注： 子作用域可以声明父作用域中的同名变量来**覆盖**父作用域中的变量。

### 总结

本篇着重讲解了**常量**，**变量**,**生命周期**与**作用域**等概念，作为cpp编程的基础，这些概念需要熟知并掌握。
