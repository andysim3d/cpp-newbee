# 流程控制

## 什么是流程控制
在我们读文章时，我们都知道应该自上向下阅读（少数语言会从右向左）。编程语言也是如此，自上向下被执行。

比如如下代码：
```cpp
std::cout<< 1 << std::endl;             // 输出 1 
std::cout<< 100 << std::endl;           // 输出 100
std::cout<< "Hello world" << std::endl; // 输出 Hello world
```
上例中的代码，会依次在命令行输出3行内容，分别为
```cpp
1
100
Hello world
```
既然我们知道cpp代码的流程，也就是执行顺序，是自上向下，那为什么还要讨论流程控制呢？

有两种情况需要流程控制：

第一种，代码中有大量重复计算，需要重复执行某些代码。

第二种，代码中有分支跳转，需要选择性执行或不执行某些代码。

## 循环

先来讨论第一种情况，假设我们现在要计算`1+2+3+...+10`的结果。你会怎么做呢？你可能会说，简单这样就可以了：
``` cpp
int a = 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10;
std::cout << a << std::endl;
```
这样固然是对的，但是一来太费键盘，二来，如果不是`1~10`,而是`1~100`，甚至`1~10000000`呢？

你可能会说，哎等差数列不是有公式嘛，我可以直接算 `(1 + n) * n / 2`就可以了。 

那……如果是让你计算`1~100000`之内所有素数的和呢？这回没有公式可以算了吧？

那么，如果我们能够把加和的部分抽离出来，重复利用，是不是就能解决“代码重复”这个问题呢？

在cpp中，想实现一个循环，我们有两种选择，`for-loop` 和`while-loop`。

### for-loop
`for-loop`的结构很简单，是
```cpp
for(<state_1>; <state_2>; <state_3>){
    // loop_body
}
```
其中，`<state_1>`会在执行`for-loop`之处执行一次，通常我们用来初始化某些变量；`<state_2>`必须为判断语句，会在每次循环执行前进行判断，如果结果为`true`，则继续循环，如果为`false`则退出循环；`<state_3>`则是对循环变量进行控制。

三个语句执行的顺序是：
第一次开始循环前：执行`<state_1>`初始化变量。
然后在每次循环开始前
- 执行`<state_2>`检查是否满足循环条件，
    - 若满足，则执行`<state_3>`更新循环变量
    - 若不满足，则结束循环

比如：

```cpp
int result = 0;                     // 初始化result变量
for (int i = 0; i <= 100; ++i){     // 开始循环
    result = result + i;            // result 每次加 i
}
```
在上例中，我们初始化了变量`result`, 然后用一个`for-loop`来计算从`0`（因为i被初始化为`0`）到`100`（因为循环继续的条件为 `i <=100`, 也即 `i=101`时结束循环）的和。

### while-loop
与`for-loop`很类似，`while-loop` 也可以用来做循环控制。但是相对简单一些，语句为
```cpp
while(<state>){
    // 循环体
}
// 或者
do {
    // 循环体
}while(<state>)
```
其中，每次进入循环前，都会检查`<state>`的值是否为`true`.若不是，则跳出循环。`do...while`的形式，则是一定执行一次`loop body`然后和普通的`while-loop`一样检查`<state>`，执行或跳出循环。

如果用`while-loop` 来计算 `0~100`的和，可以：
```cpp
// while-loop
int result = 0; 
int i = 0; 
while(i <= 100){
    result = result + i;
}
// do-while
i = 0; 
result = 0;
do{
    result = result + i;
}while (i <= 100)
```

### break 与continue
如同其字面意思，`break`可以在任意时刻立刻结束循环体。`continue`则与字面意义稍微有些不通，它是在当前为止结束本次循环，并进行下一次循环。

比如
```cpp
int i = 1;
while (true){
    i = i * 2;
    break;
}
std::cout << i << std::endl; // 输出 2

i = 0;
while (i < 5){
    i = i + 1;
    continue;
    i = i + 100;    //这行永远不会被执行到
}
std::cout << i << std::endl; // 输出 5
```

在第一个例子中，第一次循环进入后，`i`的值从1 变为2，然后遇到了`break`语句，退出循环。在下一行输出的时候，i的值就是2。 

在第二个例子中，每次进入循环，`i`增加1之后，遇到`continue`，立刻开始新的循环，所以`i = i + 100;`这行永远不会被执行到。

## 条件

### If-statement
讲完了第一种情况，我们来说说第二种情况。接着来讲讲条件。举另外一个例子，假如我们要计算`1~100`里所有奇数的和。我们已经知道了循环，那么代码可以是这样：

```cpp
int result = 0;
for (int i = 0; i <= 100; i=i+1){
    // _____请填空____
}
```
在请填空处，我们如何只让`result`与奇数的`i`相加，而不管偶数的`i`呢？

聪明的你可能想到，如果你把`i=i+1`换成`i=i+2`，那“请填空”处只要写 `result = result + i`不就可以了？

没错。任何“需求”都有无数种实现的方法。但这里，我们假设其他的代码都是固定的，你只能提供“请填空”处的代码，你该怎么办呢？

我们需要做一个判断，如果`i`是奇数，我们什么都不做，如果`i`是偶数，我们就把它加到`result`上。听起来不错对不对？

对于这种单一条件的判断，我们会使用`if-statement`。 它长这样：

```cpp
if (condition-statement){
    // 分支1
}
else {
    // 分支2
}
```
`condition-statement`是一个用来判断的条件，在这里，如果`condition-statement` 满足，则执行 分支1 块的指令，否则执行 分支2 块。

> 注意，else 在这里是可选的，即你可以有`if` 而没有与之对应的`else`语句。

这个应该很好理解，只要稍微有英语基础的就能理解`if ... else`的意思。当然，如果你有多个条件要判断，比如计算学生的绩点，我们常常会见到这样的代码
```cpp
int score; grade;
// get score input;
if (score >= 90){
    grade = 4; // A
} else if (score >= 80){
    grade = 3; // B
} else if (score >= 70){
    grade = 2; // C
} else if (score >= 60){
    grade = 1; // D
} else {
    grade = 0; // F
}
```

这个代码里，它先判断了`score`是否大于90.如果是，则进入第一个分支（100-90)。如果不是，则判断是否大于80，如是则进入第二个分支（89-80），直到覆盖所有的情况。

回到我们开始的问题，在`请填空`处填入什么合理呢？这里笔者给出一个参考答案：
```
if (i % 2 == 1){
    result = result + i;
}
```
`i % 2`是`i`对`2`进行求余的操作。如果余数是`1`，那么`i`就是奇数。通过这个方法，我们可以忽略所有偶数的`i`，达到奇数求和的目的。

除了`if-statement`之外，还有两种做条件判断的方法。一种是`switch-statement`, 另一种就是`condition-operator`条件运算符。


### Switch-statement
`if-statement`只能处理`<condition-statement>`为真或假的情况。那如果`<condition-statement>`有更多值呢？

比如`<condition-statement>`是范围1～10的数字，而我们要就每种情况进行不通逻辑的处理。当然读者可能说用刚刚讲到的 `if-else if-else`串来写，那代码看起来很丑，所以有了`Switch-statement`来进行多分支处理。 

`Switch-statement` 的语法是：

```cpp
switch(<condition-statement>){
    case condition-A:
        branch-A;
        break;
    case condition-B
        branch-B;
        break;
    ...
    case condition-X
        branch-X;        
        break;
    default:
        branch-default;
}
```

### Condition-operator

`condition-operator` 的用法是： `<condition-statement>?<branch-A>:<branch-B>`。如果`<condition-statement>`中条件为真，则执行`<branch-A>`，否则执行`<branch-B>`. 

仔细观察后读者可能会发现和`if-statement` 很像，唯一的不同是`if-statement` 的 `else`语句可以省略，而`condition-operator`不能省略`<condition-statement>`不为真的分支。
