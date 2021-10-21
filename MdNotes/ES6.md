# ECMAScript

ECMAScript 只提供了最基本的语法。

JavaScript是ECMAScript的实现和扩展。

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210515172627003.png" alt="image-20210515172627003" style="zoom:50%;" />

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210515172651627.png" alt="image-20210515172651627" style="zoom:50%;" />

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210515173426311.png" alt="image-20210515173426311" style="zoom:50%;" />



从ES2015 后，ECMA组织决定不再按照版本号而是按照发行**年份**命名，但是习惯上，大家依然称呼**ES2015为 ES6**（沿着以前的ES5版本顺序）。

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210515174310217.png" alt="image-20210515174310217" style="zoom:50%;" />

https://262.ecma-international.org/6.0/





很多开发中用ES6**泛指**从ES2015之后所有的新标准。

比如大家说“ES6 中的async/await”，其实这是ES2017中指定的标准。



ES5.1基础之上的变化：

1. 解决原有语法上的一些问题或者不足
2. 对原有语法进行增强
3. 全新的对象，全新的方法，全新的功能
4. 增加新的数据结构、类型 （Symbol Map 等）





# ES6 环境搭建

## 简介

### Babel 转码器

- babel 转码器 是一个ES6 转码器

- 配置文件.babelrc(所有babel工具和模块的使用，都必须先写好此文件)

  - ```javascript
    {
      "presets": [// 设置转码规则
        "@babel/env",
        "@babel/preset-react"
      ], 
      "plugins": [] 
    }
    ```

npm 安装babel

node 是一个服务器环境



# 作用域 与 变量声明



- 全局作用域
- 函数作用域
- 块级作用域 （新增）



块级作用域：{} 如 if语句， for语句



## let 与 var

- let 声明的范围是**块级作用域**，而var是函数作用域
  - （在迭代循环中用let，里层循环不会影响外层循环的变量）
- let 在一个作用域范围内不允许出现**冗余声明**
- 暂时性死区
  - let 声明的变量**不会在作用域中被提升**
- 全局声明
  - **let 在全局作用域中声明的变量不会成为 window 对象的属性**（可以避免造成全局对象污染） （var 会）

为什么ecma不直接在var的基础上进行改善修正？

（避免之前的许多项目无法工作）



## const

在let基础上，增加了 只读 特性，声明过后不允许再被**修改**，声明时必须**初始化**变量。

const声明的只读限制，只适用于它指向的变量的引用。如果const变量引用的是一个对象，修改这个对象的属性及值是允许的。但对对象变量本身重新赋值是不可以的。



## 变量冻结 Object.freeze

常量声明的对象是可以修改其属性及值的，因为变量声明时开辟的内存空间对应的指针没有改变，修改属性只是修改这块空间里的属性及值。

如果一个常量对象不想被修改属性，就使用Object.freeze进行焊死。而且，配合`'use strict'` 使用会进行报错提醒 



## 声明变量的最佳实践

**不用var，主用const, 配合let**



## 传值与传址

基本数据类型的变量赋值给变量的操作，一般是传值，把变量的值复制给另一个变量。两个变量值的改变不会再互相影响。



而针对内存比较大的数据类型或操作，不会进行数据的复制，而是把数据的内存地址给到另一个变量，叫传址，这时两个变量对数据的操作修改是会影响到两个变量的，因为修改的是两个变量都指向的内存地址里的数据。

跟 深拷贝 浅拷贝 有关？

```javascript
 //基本数据类型的变量赋值
            let a = 2;
            let b = a;
            console.log(a, b); //2 2
            //改变其中一个变量的值, 不会相互影响
            b = 6;
            console.log(a, b); //2 6

            //复杂数据或内存空间较大的数据
            let obj = { uname: 'fei', age: 30 };
            let obj2 = obj;
            console.log(obj, obj2);//{uname: "fei", age: 30} {uname: "fei", age: 30}
            //修改其中一个变量的值，两个变量的数据都会改变
            obj2.uname = 'neo';
            console.log(obj, obj2);//{uname: "neo", age: 30} {uname: "neo", age: 30}
```



## null 和 undefined

```javascript
//基本类型 用 undefined
//引用类型 用 null
```





## ES6 声明变量的六种方法

- var
- function
- let
- const
- import
- class





# ES6 解构赋值

**Destructuring**



## 数组解构赋值

```javascript
// 解构赋值语法
// 解构赋值语法是一种 Javascript 表达式。通过解构赋值，可以将属性/值从对象/数组中取出，赋值给其他变量。
const arr = ['red', 'orange', 'yellow', 'green'];

/* const first = arr[0];
const second = arr[1];
//... */

//使用解构 快速赋值
const [firstColor, secondColor, ...restColor] = arr;
console.log(firstColor, secondColor, restColor); //red orange [ 'yellow', 'green' ] 

// 只需要获取第3个成员
//保证解构位置与数组一致
const [, , likeColor] = arr;
console.log(likeColor); //yellow
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



## 对象解构赋值

对象里的属性成员没有固定顺序位置，不能按照下标索引取值。

```javascript
const obj = {
    uname: 'neo',
    age: 30,
    gender: 'male'
}

//利用对象解构赋值语法
//声明变量 myName myAge 分别赋值 对象obj中的uname属性和age属性的值
const { uname: myName, age: myAge } = obj;

console.log(myName, myAge); //neo 30
```



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



# ES6 字符串

## 模板字符串

templates strings

``

- 可以直接输入换行符
- 插值表达式嵌入变量值，方便直观 ${}



### 带标签的模板字符串？？

tagged templates

作用：

- 对模板字符串进行加工





## 字符串的扩展方法

- includes
- startsWith()
- endsWith()

以上三个方法全部返回**布尔值**







# ES6 函数

## 参数默认值

```javascript
//以前设置参数的默认值
function myFunc(enable) {
  enable = enable === undefined ? true : enable; //设置默认值为 true
  console.log(enable);
}

myFunc(false); //false
myFunc(); //true, 无值时为默认值 true


//ES6 新增方法
//直接在参数里设置默认值

function myFunc2(enable = true) {
    console.log(enable);
}
myFunc2() //true
myFunc2(false) //false
```



## 剩余参数

剩余模式

```javascript
//以前使用arguments
function foo() {
  console.log(arguments)
}

//使用剩余模式
//只能在参数的最后使用
function foo(...args) {
  console.log(args)
}
```



## 展开操作符





## 箭头函数



简洁 易读



### this

箭头函数中没有this，不会改变this的指向，箭头函数外层的this









# ES 对象

## 对象字面量的增强

计算属性名









# ES6 Symbol









# ES6 Map 与 Set





# ES6 Reflect 与Proxy





# ES6 字符串







