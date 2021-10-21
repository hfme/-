



# 介绍





# 语法与数据类型

语法

注释

变量

变量的命名规则

变量声明

let var const区别

作用域

变量提升

函数提升

全局变量

字面量

数据结构与类型

数据类型转换

字面量

数组字面量、布尔字面量、浮点数字字面量、整数字面量、对象字面量、**增强的对象字面量？？**、RegExp字面量、字符串字面量

特殊字符&转义字符

## 数据类型：8种

### 7种基本数据类型：

### 1种复杂数据类型



**对象和函数是JS语言的另外两个基本元素。**

**对象可以看作存放值的容器，函数可以当作程序执行的步骤。**

在JS中，几乎所有的对象都是Object类型的实例，都会从Object.prototype继承属性和方法。

每个函数实际上都是一个Function对象。(函数就是构造函数Function的实例对象。)





数据类型转换

JS语言是一种 **动态类型** 语言，声明时可以不指定数据类型，数据类型会在代码执行时根据需要自动转换。













# 流程控制与错误处理

- if...else
- switch
- try/catch/throw
- Error 对象





## 语句块

 {}   

（延伸：块作用域 let const）



## 条件判断语句

### **if...else...**

被看做false的值：false、undefined、null、0、NaN、" " 空字符串



延伸：布尔值 与 Boolean对象 不能搞混

Boolean对象：布尔值的对象容器。除了Boolean(undefined) 或 Boolean(null) 会被看做false外，值为其他包括false的Boolean对象，传递给条件语句时都将计算为true.

```javascript
var x = new Boolean(false); //Boolean对象的计算结果是true
if (x) {
  // 这里的代码会被执行
}
```

不要用创建 `Boolean` 对象的方式将一个非布尔值转化成布尔值，直接将 `Boolean` 当做转换函数来使用即可，或者使用[双重非（!!）运算符](https://developer.mozilla.org/zh-CN/docs/conflicting/Web/JavaScript/Reference/Operators_f71733c8e7001a29c3ec40d8522a4aca#逻辑非（!）)：

```javascript
var x = Boolean(expression);     // 推荐
var x = !!(expression);          // 推荐
var x = new Boolean(expression); // 不太好
```



### **switch语句**



## 异常处理语句

用throw语句抛出异常，用try...catch语句捕获异常并处理

### **异常类型** 

Error

通过Error构造器可以创建一个错误对象。当运行时出现错误，Error的实例对象就会被抛出。

Error对象也可用于用户自定义的异常的基础对象。

例子：

```javascript
//抛出一个基本错误
//使用 throw 关键字来抛出创建的Error对象，使用try...catch...结构处理异常
try {
  throw new Error("Wrong!");
} catch (e) {
  alert(e.name + ":" + e.message);
}

```

更多 Error 相关：

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error

DOM Exception

DOM Exception 接口代表调用方法或访问Web API 属性时发生的异常事件。

**`DOMException()`** 构造函数返回一个包含指定的信息和名称的 `DOMException` 对象。

DOMError 已废弃





### throw语句



### try...catch 语句

 catch块

finally 块

嵌套 try...catch 语句



### 使用Error对象





## Promises

ES6 增加了对 Promise 对象的支持，它允许对**延时**和**异步操作流进行控制**。

**Promise** 

Promise 对象代表了一个异步操作的最终完成或者失败。

JS中的Promise代表的是已经正在发生的进程，而且可以通过回调函数实现链式调用。

（延伸：什么是惰性求值？）

resolved?

pending

fulfilled

rejected

![promises](/Users/neofei/Fei/Frontend/笔记/images/promises.png)

### Promise 的链式调用







放着 放着 难理解 ...







# 循环与迭代

### for 语句

### do...while 语句

### while 语句



### label 语句

一个 [`label`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/label) 提供了一个让你在程序中其他位置引用它的标识符。

你可以用 label 标识一个循环， 然后使用 `break` 或者 `continue` 来指出程序是否该停止循环还是继续循环。

```javascript
//有了标签，可以使用break和continue在多层循环的时候控制外层循环。
//例如下面：
outerloop:
for (var i = 0; i < 10; i++)
{
    innerloop:
    for (var j = 0; j < 10; j++)
    {
        if (j > 3)
        {
            break;
        }
        if (i == 2)
        {
            break innerloop;
        }
        if (i == 4)
        {
            break outerloop;
        }
        document.write("i=" + i + " j=" + j + "");
    }
}

```



### break 语句

### continue 语句

### for...in语句

[`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 语句循环一个指定的变量来循环一个对象所有可枚举的属性。

### for...of 语句

[`for...of`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of) 语句在[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)（包括[`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)、[`Map`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)、[`Set`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)、[`arguments`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments) 等等）上创建了一个循环，对值的每一个独特属性调用一次迭代。









# 函数



### 定义函数

1. function 函数声明 （函数提升）
2. 函数表达式（不会进行函数提升）
3. 用Function构造器创建函数



### 调用函数

函数的定义要在调用函数所在的作用域内。

普通调用。

**递归**：函数可以调用其本身。

**阶乘**(factorial)：正整数的阶乘等于所有小于及等于该整数的乘积，且0的阶乘是1。自然数（0和正整数）的阶乘可以写作n!

- $$
  阶乘公示： n! = 1*2*3*...*(n-1)*n
  $$

- $$
  递归方式： 0！= 1, n! = (n-1)! * n
  $$

  

```javascript
//用函数实现阶乘
function factorial(n) {
  if(n==0 || n==1) {
    return 1;
  } else {
    return n*factorial(n-1);
  }
}
```



还有apply() call() bind()等调用方式，这几个方法的作用除了调用函数，主要目的在于改变函数的this指向，并传递参数。

apply() call() bind() 的区别





### 函数作用域

在函数内声明的变量不能在函数之外访问。



### 作用域和函数堆栈

递归函数就使用了堆栈：函数堆栈

#### 递归

#### 嵌套函数和闭包

#### 保存变量

#### 多层嵌套函数

#### 命名冲突



### 闭包



### 使用arguments 对象



### 函数参数

#### 默认参数

#### 剩余参数



### 箭头函数





### 预定义函数（内置函数）





# 表达式与运算符



#### 解构赋值

**解构赋值语法**是一种JavaScript表达式。通过解构赋值，可以将属性/值从对象/数组中取出，赋值给其他变量。



```javascript
let a, b, rest;
[a, b] = [10, 20];
// ...rest 是剩余模式，将数组剩余部分赋值给一个变量
[a, b, ...rest] = [10, 20, 30, 40, 50, 60];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50, 60] 
```



对象和数组逐个对应表达式，提供了一种简单的定义一个特定的数据组的方法。

解构赋值表达式左边定义了要从原变量中取出什么变量。

```javascript
let x = [1, 2, 3, 4, 5];
let [y, z] = x; // 这里解构赋值其实就是把 变量x 的前两个值给 y z
console.log(y); // 1
console.log(z); // 2 
```





##### 解构数组

```javascript
let arr = ['one', 'two', 'three'];
let [str1, str2, str3] = arr;
console.log(str1); // 'one'
console.log(str2); // 'two'
console.log(str3); // 'three'
```



**默认值**

为了防止从数组中取出一个值为`undefined`的对象，可以在表达式左边的数组中为任意对象预设默认值。

```javascript
var a, b;

[a=5, b=7] = [1];
console.log(a); // 1
console.log(b); // 7
```



**交换变量**

没有解构赋值时，交换两个变量需要定义一个临时变量，解构赋值可以直接完成交换。

```javascript
let a = 1;
let b = 2;
[a, b] = [b, a]; //完成交换
```



**解析从函数返回的数组**

```javascript
function f() {
  return [1, 2];
}

var a, b;
[a, b] = f();
console.log(a); // 1
console.log(b); // 2
```



**剩余模式**

如上已用到，解构数组时可以使用剩余模式，将数组剩余部分赋值给一个变量。

注意：剩余元素必须是最后一个元素，否则会抛出 syntaxError





##### 解构对象

基本赋值

```javascript
var o = {p: 42, q: true};
var {p, q} = o;

console.log(p); // 42
console.log(q); // true
```

无声明赋值

```javascript
var a, b;
({a, b} = {a: 1, b: 2}); //这里前面没有声明，注意：
// 对象字面量无声明解构赋值，必须使用()包裹；且前一语句必须使用分号，避免（）被当成函数
```











# Number对象 Math对象 Date对象 String对象 字面量 正则表达式





# 数组



# 带键集合



# 处理对象





# 对象模型的细节





# Promise





# 迭代器与生成器





# 元编程





# 模块

