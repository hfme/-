# 基础知识

## JavaScript 与 ECMAScript

JavaScript 远不止ECMAScript，ECMAScript 只提供了最基本的语法。

JavaScript是ECMAScript的实现和扩展。

JavaScript 包含 ECMAScript、DOM、BOM。



<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210601124945589.png" alt="image-20210601124945589" style="zoom:50%;" />





<img src="/Users/neofei/Fei/Frontend/笔记/images/065468E7-C355-499D-880F-D501E592DC00.png" alt="065468E7-C355-499D-880F-D501E592DC00" style="zoom: 60%;" />

<img src="/Users/neofei/Fei/Frontend/笔记/images/DF53F323-BF58-47B3-A01B-D149E7761E5F.png" alt="DF53F323-BF58-47B3-A01B-D149E7761E5F" style="zoom:50%;" />



扩展：Node.js

Node.js 是一个平台或者工具

后端的JavaScript = ECMAScript + IO + File +... 等服务器端的操作

npm：node包管理工具



## 作用域 与 变量



- 全局作用域
- 函数作用域
- 块级作用域 （新增）



块级作用域：{} 如 if语句， for语句



### let 与 var

- let 声明的范围是**块级作用域**，而var是函数作用域
  - （在迭代循环中用let，里层循环不会影响外层循环的变量）
- let 在一个作用域范围内不允许出现**冗余声明**
- 暂时性死区
  - let 声明的变量**不会在作用域中被提升**
- 全局声明
  - **let 在全局作用域中声明的变量不会成为 window 对象的属性**（可以避免造成全局对象污染） （var 会）

为什么ecma不直接在var的基础上进行改善修正？

（避免之前的许多项目无法工作）



### const

在let基础上，增加了 只读 特性，声明过后不允许再被**修改**，声明时必须**初始化**变量。

const声明的只读限制，只适用于它指向的变量的引用。如果const变量引用的是一个对象，修改这个对象的属性及值是允许的。但对对象变量本身重新赋值是不可以的。



### 变量冻结 Object.freeze

常量声明的对象是可以修改其属性及值的，因为变量声明时开辟的内存空间对应的指针没有改变，修改属性只是修改这块空间里的属性及值。

如果一个常量对象不想被修改属性，就使用Object.freeze进行焊死。而且，配合`'use strict'` 使用会进行报错提醒 



### 声明变量的最佳实践

**不用var，主用const, 配合let**



## 传值与传址

**基本数据类型**是指 数字型、字符串等简单数据类型，**引用类型**指对象数据类型。

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







![image-20210805171504163](/Users/neofei/Fei/Frontend/笔记/images/image-20210805171504163.png)





### ES6声明变量的六种方法

var, function, let, const, import, class





# 运算符与流程控制

## 运算符

### 基础运算符

**运算元**：运算符应用的对象。或者称其为‘参数’

**一元运算符**：只有一个元算元 

 1. 一元运算符 `+`

    - **数字转化**

      - 加号 `+` 应用于单个值，对数字没有任何作用

      - 如果**运算元不是数字**，加号 `+` 则会将其**转化为数字**。

    - ```js
      // 对数字无效
      let x = 1;
      alert( +x ); // 1
      
      let y = -2;
      alert( +y ); // -2
      
      // 转化非数字
      alert( +true ); // 1
      alert( +"" );   // 0
      ```

      ```js
      let apples = "2";
      let oranges = "3";
      
      // 在二元运算符加号起作用之前，所有的值都被转化为了数字
      alert( +apples + +oranges ); // 5
      
      // 更长的写法
      // alert( Number(apples) + Number(oranges) ); // 5
      ```

      

 2. 一元负号运算符`-`

    - 它的作用是对数字进行**正负转换**：

    - ```js
      let x = 1;
      
      x = -x;
      alert( x ); // -1，一元负号运算符生效
      ```

      

**二元运算符**：有两个运算元

 1. 加、减、乘、除、取余、求幂(**) 等用于两个数的计算

 2. 二元运算符`+`， **拼接字符串符号**

    - 如果加号 `+` 被应用于**字符串**，它将合并（连接）各个字符串

    - 只要任意一个**运算元是字符串**，那么另一个运算元也将被转化为字符串

    - ```js
      "$" + 4 + 5; //'$45'
      ```

    其他算术运算符只对数字起作用，并且总是将其**运算元转换为数字**。







### 运算符优先级

1. `()`圆括号拥有最高的优先级
2. **一元**运算符优先级**高于二元**运算符
3. 逻辑运算符中，逻辑 `!` 的优先级最大，其次是逻辑`&&`，然后逻辑`||`
4. `??`运算符优先级很低，仅在`=`和`?`之前计算，在大多数运算符之后计算。



### 比较运算符

所有比较运算符均**返回布尔值**。

**字符串比较**：

在比较字符串的大小时，JavaScript 会使用“字典（dictionary）”或“词典（lexicographical）”顺序进行判定。

字符串是按字符（母）逐个进行比较的。

字符串的比较算法非常简单：

1. 首先比较两个字符串的首位字符大小。
2. 如果一方字符较大（或较小），则该字符串大于（或小于）另一个字符串。算法结束。
3. 否则，如果两个字符串的首位字符相等，则继续取出两个字符串各自的后一位字符进行比较。
4. 重复上述步骤进行比较，直到比较完成某字符串的所有字符为止。
5. 如果两个字符串的字符同时用完，那么则判定它们相等，否则未结束（还有未比较的字符）的字符串更大。

**非真正的字典顺序，而是 Unicode 编码顺序**

在上面的算法中，比较大小的逻辑与字典或电话簿中的排序很像，但也不完全相同。

比如说，字符串比较对字母大小写是敏感的。大写的 `"A"` 并不等于小写的 `"a"`。哪一个更大呢？实际上**小写的 `"a"` 更大**。这是因为在 JavaScript 使用的内部编码表中（Unicode），小写字母的字符索引值更大。



**不同类型间的比较**

当对不同类型的值进行比较时，JavaScript 会首先将其转化为数字（number）再判定大小。

```js
alert( '2' > 1 ); // true，字符串 '2' 会被转化为数字 2
alert( '01' == 1 ); // true，字符串 '01' 会被转化为数字 1
```

对于布尔类型值，`true` 会被转化为 `1`、`false` 转化为 `0`。

```js
alert( true == 1 ); // true
alert( false == 0 ); // true
```



**严格相等**

普通的相等性检查 `==` 存在一个问题，它不能区分出 `0` 和 `false`，也同样无法区分空字符串和 `false`

**严格相等运算符 `===` 在进行比较时不会做任何的类型转换。**



#### **对 null 和 undefined 进行比较**

它们俩就像“一对恋人”，**仅仅等于对方而不等于其他任何的值**（只在非严格相等下成立）。

```js
//严格比较
alert( null === undefined ); // false

//非严格相等
alert( null == undefined ); // true
```

当使用数学式或其他比较方法 `< > <= >=` 时：

`null/undefined` 会被转化为数字：`null` 被转化为 `0`，`undefined` 被转化为 `NaN`。

`undefined` 和 `null` 在相等性检查 `==` 中不会进行任何的类型转换。 



### 逻辑运算符

#### 逻辑或`||  `  逻辑`&& `  逻辑非`!`

大多数情况下，逻辑或 `||` 会被用在 `if` 语句中，用来测试是否有 **任何** 给定的条件为 `true`。

拓展算法：

逻辑或 `||` 寻找第一个**真值**，返回这个**值**，如果没有真值，就返回**最后一个值**。

逻辑与`&&` 寻找第一个假值，返回这个**值**，如果没有假值，就返回**最后一个值**。

逻辑非`!` 将操作数转化为布尔类型true/false, 返回相反的布尔值。通常使用两个`!!`用来将某个值转化为布尔类型。使用`Boolean()` 也可以。

PS： 对alert()的调用没有返回值，其返回值是`undefined`



##### 短路运算

```javascript
// 短路运算
//利用 逻辑或 || 写短路运算
// 如果用户没有输入那么gender 值为 保密
let gender = prompt('请输入性别') || '保密'; //书写顺序很重要 因为是或
console.log(gender);

```

```javascript
//后盾向军老师选择DOM元素的常用方法，挺好玩儿的，模仿一下
function query(el) {
	return document.querySelector(el);
}

//给form添加提交事件
query("form").addEventListener('submit', formCheck);
function formCheck(e) {
	let uname = query('[name="uname"]').value.trim();
	let copyright = query('[name="copyright"]').checked;
	e.preventDefault(); //加上这句话，表单不会被清空，阻止form的默认事件
  console.log(uname, copyright);
  if (!uname || copyright === false) {
  	alert('mmmm')
  }
}
```





### 空值合并运算符`??`

通常，`??` 的使用场景是，为可能是未定义的变量提供一个默认值。

如果第一个参数不是 `null/undefined`，则 `??` 返回第一个参数。否则，返回第二个参数。



**`??` 和 `||`** 

它们之间重要的区别是：

- `||` 返回第一个 **真** 值。
- `??` 返回第一个 **已定义的** 值。

`||` 无法区分 `false`、`0`、空字符串 `""` 和 `null/undefined`。它们都一样 —— 假值（falsy values）。

```js
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```



`??`的优先级很低，有必要时一定要加上`()`圆括号使用。

- 如果没有明确添加括号，不能将其与 `||` 或 `&&` 一起使用。

```js
let height = null;
let width = null;

// 重要：使用括号
let area = (height ?? 100) * (width ?? 50);

alert(area); // 5000
```







## 流程控制

### label 与 continue 和 break

**label**

标签相当于定位符，用于跳转到程序的任意位置。

标签可以是任意的标识符，但不能是保留字。

标签通常与 `break`语句 和 `continue`语句配合使用，跳出特定的循环。

```js
label:
	语句
```





```javascript
//break 退出整个循环
            //continue 退出当前循环 继续下一个循环
            //label 给循环加上label 可以通过label操作循环的继续或退出

            outter: for (let i = 0; i < 6; i++) {
                inner: for (let j = 0; j < 6; j++) {
                    console.log(i, j);
                    if (i + j > 10) {
                        break outter;
                    }
                }
            }
```

### for...in 与 for...of

```javascript
//for...in 遍历对象
      //for...of 遍历数组、字符串等原型上有迭代对象的数据类型
      let blogs = [
        { title: "走进JS高阶函数", article: 8, content: "balabal" },
        { title: "走进JS面向对象", article: 9, content: "balabal" },
      ];

      for (let k of blogs) {
        console.log(k);
        console.log(k.title);
      }

      // 使用 forEach & 箭头函数 真够简洁的
      blogs.forEach((value) => console.log(value));

      blogs.forEach((value, index) => {
        console.log(index);
        console.log(value.title);
      });
```



# 基本数据类型

## 概述

JavaScript语言的每一个值，都属于某一种数据类型。

JavaScript中共有**8种基本的数据类型**（**7种基本类型**和**1种复杂数据类型**）：

1. 数值，number，整数和小数
2. 字符串，string，文本
3. 布尔值，boolean，true/false
4. 任意精度的整数，BigInt，可以安全的存储和操作大整数。
5. undefined, 用于未定义的值
6. null，表示空值
7. symbol，其实例是唯一且不可改变的数据类型，用于唯一的标识符
8. 对象，object，各种值组成的集合（复杂数据类型）



通常，nubmer, string, boolean 这三种类型，被称为**原始类型**。

undefined 和 null 一般被看做两个特殊值。

**对象**往往是多个原始类型的值的集合，可以看做一个**存放各种值的容器**。

对象又可以分为三个子类型：

- 狭义的对象，object
- 数组，array
- 函数，function



函数其实就是处理数据的方法，JavaScript把它当作一种数据类型，可以赋值给变量，灵活，为函数式变成奠定基础。

MDN备注：

**对象和函数是JS语言的另外两个基本元素。**

**对象可以看作存放值的容器，函数可以当作程序执行的步骤。**

在JS中，几乎所有的对象都是Object类型的实例，都会从Object.prototype继承属性和方法。

每个函数实际上都是一个Function对象。(函数就是构造函数Function的实例对象。)



**数据类型转换**

JS语言是一种 **动态类型** 语言，声明时可以不指定数据类型，数据类型会在代码执行时根据需要自动转换。



## 类型检测

学会使用最适合的数据类型处理业务。

### typeof

可以用于检测：

- 基本数据类型：number, string, boolean
- function
- object
- undefined

但是用typeof检测Array, null, Object等都会返回object，无法区分。

```js
let num = 1;
console.log(typeof num); //'number'
```



### instanceof

instanceof 运算符用于检测构造函数的prototype属性是否出现在某个实例对象的原型链上。可以理解为，是否是某个对象的实例。

```javascript
let arr = [];
let obj = {};
//这里使用instanceof检测可以理解为：arr 是否是 Array 的实例化对象 arr instanceof Array
console.log(arr instanceof Array); //true, 表明arr是Array
console.log(obj instanceof Array); //false

console.log(obj instanceof Object); //true

let fn = function () {};
console.log(fn instanceof Function); //true
```



### Object.prototype.toString







## null 和 undefined

```javascript
//基本类型 用 undefined
//引用类型 用 null
```

对声明但未赋值的变量返回类型为**udefined**, 表示值未定义。

**null** 用于定义一个空对象，如果变量要用来保存引用类型，可以在初始化的时候将其设置为null。



**区别**：

`null`是一个表示“空”的对象，转为数值时为**`0`**；`undefined`是一个表示"此处无定义"的原始值，转为数值时为**`NaN`**。



```js
      //undefined的典型场景：
      //1. 变量声明了但没有赋值
      let a;
      console.log(a); //undefined
      var b;
      console.log(b); //undefined

      //2. 调用函数时没有提供应该提供的参数
      function fn(x) {
        //这里声明函数时相当于声明了一个变量x来接收参数，但调用时没有参数，最后就返回了undefined，和上面声明了变量但没有赋值类似
        return x;
      }
      console.log(fn()); //undefined 调用函数时没有提供应该提供的参数

      //3.对象里的属性没有赋值
      let obj = {};
      obj.name; //这里给对象obj追加了一个属性name，但是没有赋值
      console.log(obj.name); //undefined

      //4.函数没有返回值时，默认返回undefined
      function hi() {}
      console.log(hi()); //undefined
```





## String





### 模板字符串

templates strings

``

- 可以直接输入换行符
- 插值表达式嵌入变量值，方便直观 ${}

```javascript
//后盾向军老师写代码看着很带劲
//他用模板字符串写了一个东西 很向Vue里面的v-for的风格

let blogs = [
	{ title: "走进JS面向对象", author: "Fei", classify: "JS" },
	{ title: "走进JS高阶函数", author: "Fei", classify: "JS" },
   { title: "走进异步JS", author: "Fei", classify: "JS" 	},
];

function template() {
//你得记着要返回值啊！要return啊
   return `<ul>${blogs
// 模版字符串里还可以嵌套使用模板字符串
   .map((value) => `<li>${value.title}</li>`)
    .join("")}</ul>`; //神奇 这里map完 又join 合并
}
document.body.innerHTML = template();
```



### 带标签的模板字符串

tagged templates

**标签模板**是提取出普通字符串与变量，交由标签函数处理。

作用：

- 对模板字符串进行加工



案例：

```javascript
  //标签模版

  let blogs = [
    { title: "走进JS面向对象", author: "Fei", classify: "JS" },
    { title: "走进JS高阶函数", author: "Neo", classify: "JS" },
    { title: "走进异步JS", author: "Harry", classify: "JS" },
  ];

  tag`博客标题${blogs.map((value) => value.title)}，作者：${blogs.map(
    (value) => value.author
  )}`;

  function tag(strings, title, author) {
    console.log(strings);
    console.log(title);
    console.log(author);
  }

  //或者使用剩余模式
  function myTag2(strings, ...args) {
    console.log(strings);
    console.log(args);
  }
  myTag2`博客标题${blogs.map((value) => value.title)}，作者：${blogs.map(
    (value) => value.author
  )}`;

  //标签模版应用案例：
  //给所有的某个关键词加上超链接，比如把所有的JS加上链接
  function template() {
    return `<ul>${blogs
      .map(
        (value) =>
          linksTag`<li>博客标题：${value.title}，博客作者：${value.author}，博客分类：${value.classify}</li>`
      )
      .join("")}</ul>`;
  }

  function linksTag(strings, ...vars) {
    // console.log(strings);
    // console.log(vars);
    return strings
      .map((str, index) => {
        return (
          str +
          (vars[index]
            ? vars[index].replace("JS", `<a href='#'>JS</a>`)
            : "")
        );
      })
      .join("");
  }
  document.body.innerHTML = template();
```



### 字符串方法

去除空白： trim(), trimLeft(), trimRight()

获取单字符：charAt(index) 或者直接 str[index]

截取字符串：slice(), substr(), substring()

查找字符串：

- 从开始位置找：indexOf('a', 1); 从某个位置开始查找，检测不到返回-1
- 从末尾开始找： lastIndexOf()
- search(), 用于检索字符串中指定的子字符串，也可以使用正则表达式搜索

- includes
- startsWith()
- endsWith()

替换字符串：replace()

重复生成： repeat()



### 类型转换

字符串转换为数组：split('')

隐式转换：

```javascript
let num = 11;
let str = num +'a';
console.log(str); //String
```



转换为字符串： toString()





### 去除字符串中空白的办法

```js
      //去除两端的空白 trim()
      let str = " hello,   world!  ";
      console.log(str);
      console.log(str.trim());

      //trim()只能去除两端空白

      //去除中间空白
      //split 转成数组 再 join
      console.log(str.split(" "));
      console.log(str.split(" ").join(""));

      //最强大，正则表达式
      //有制表符的字符串，使用split 也不行
      let str1 = " \t hello,  world!  ";
      console.log(str1);
      console.log(str1.split(" ").join("")); //前面的制表符还是没有删除掉
      console.log(str.replace(/\s+/g, "")); //已删除掉了所有空白
      //\s（“s” 来自 “space”）
      // 空格符号：包括空格，制表符 \t，换行符 \n 和其他少数稀有字符，例如 \v，\f 和 \r。

      //修饰符：
      //i 不区分大小写
      //g 查找所有的匹配项，而不是第一个
      //m 多行模式
```



## Boolean

`let bl = true;`

### 隐式转换

基本上所有类型都可以隐式转换为 Boolean类型。

| 数据类型  | true                 | false            |
| --------- | -------------------- | ---------------- |
| String    | 非空字符串           | 空字符串         |
| Number    | 非0的数值            | 0 、NaN          |
| Array     | 数组**不参与比较时** | 参与比较的空数组 |
| Object    | **所有对象**         |                  |
| undefined | 无                   | undefined        |
| null      | 无                   | null             |
| NaN       | 无                   | NaN              |

当与boolean类型比较时，会将两边类型统一为数字1或0。



注意，空数组（`[]`）和空对象（`{}`）对应的布尔值，都是`true`。

```js
if ([]) {
  console.log('true');
}
// true

if ({}) {
  console.log('true');
}
// true
```





## Number

### 概述

JS中，所有数字都是以**64位浮点数**形式存储，即使是整数也是如此。

这就是说，**JS语言的底层根本没有整数**，所有数字都是小数（64位浮点数）。（为什么这么设计？？）

由于浮点数不是精确的值，所以涉及小数的比较和运算要特别小心。

```js
      //JS 底层根本没有整数！！？
      //所有数字都是64位浮点小数，包括整数也是以小数形式存储的
      let num = 2;
      console.log(2);
      console.log(2 === 2.0); //true

      //浮点数的计算，由于精确度的问题，要特别的小心
      console.log(1.1 + 2.2 === 3.3); //false
      console.log(1.1 + 2.2); //3.3000000000000003

      console.log(1.1 + 1.1); //2.2
      console.log(2.2 + 2.1); //4.300000000000001
      //0.3-0.2并不等于0.1  啊啊啊
      console.log(0.3 - 0.2); //0.09999999999999998
```





**数值的进制**：常见的有二进制、八进制、十进制、十六进制

- 八进制：有前缀`0o`或`0O`的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值。
- 十六进制：0～9 a~f , 数字的前面加0x 表示十六进制
- 二进制：有前缀`0b`或`0B`的数值



**数值范围**

JavaScript 能够表示的数值范围为**2^1024^到2^-1023^**（开区间），超出这个范围的数无法表示。

- 最大值：Number.MAX_VALUE;  (1.7976931348623157e+308)
- 最小值：Number.MIN_VALUE;   (5e-324)

数字型**特殊值**：

- infinity, 无穷大 (JS无法表示大于2^1024^的数， 就会发生正向溢出，会返回`Infinity`)

- -infinity，无穷小

- NaN，Not a Number，代表一个非数值

  - 主要出现在将字符串解析成数字出错的场合

  - ```js
    
    5 - 'x' //NaN
    
    // 0 除以 0 也会得到 NaN
    0 / 0 //NaN
    
    //运算规则
    //NaN 不等于任何值，包括它本身
    NaN === NaN //false
    
    
    //NaN 与任何数的运算得到的都是NaN
    ```

  - **isNaN()** 

    - 判断是否是非数字，返回一个值，是数字返回false，不是数字返回true

- **正零和负零**

  - JavaScript 内部实际上存在2个`0`：一个是`+0`，一个是`-0`

    ```js
    (1 / +0) === (1 / -0) // false
    ```

    







### 数值类型的全局方法

**Number** 

转换为数字型

```javascript
console.log(Number('houdunren')); //NaN
console.log(Number(true));	//1
console.log(Number(false));	//0
console.log(Number('9'));	//9
console.log(Number([]));	//0
console.log(Number([5]));	//5
console.log(Number([5, 2]));	//NaN
console.log(Number({}));	//NaN
```



**parseInt**

- 字符串转换为整数

- 字符串转为整数的时候，是一个个字符依次转换，如果遇到不能转为数字的字符，就不再进行下去，返回已经转好的部分。
- 如果字符串的**第一个字符**不能转化为数字（后面跟着数字的**正负号**除外），返回`NaN`。
- **进制转换**
  - `parseInt`方法还可以接受第二个参数（**2到36**之间），表示被解析的值的进制，返回该值对应的十进制数。**默认情况**下，`parseInt`的第二个参数为10，即默认是十进制转十进制。
  - 如果第二个参数不是数值，会被自动转为一个整数。这个整数只有在2到36之间，才能得到有意义的结果，超出这个范围，则返回`NaN`。如果第二个参数是`0`、`undefined`和`null`，则直接忽略。

```js
parseInt('10', 37) // NaN
parseInt('10', 1) // NaN
parseInt('10', 0) // 10
parseInt('10', null) // 10
parseInt('10', undefined) // 10
```





**parseFloat**

-  字符串转换为浮点数
- 如果参数不是字符串，或者字符串的第一个字符不能转化为浮点数，则返回`NaN`。



**isFinite()**

`isFinite`方法返回一个布尔值，表示某个值是否为正常的数值。

```js
isFinite(Infinity) // false
isFinite(-Infinity) // false
isFinite(NaN) // false
isFinite(undefined) // false
isFinite(null) // true
isFinite(-1) // true
```

除了`Infinity`、`-Infinity`、`NaN`和`undefined`这几个值会返回`false`，`isFinite`对于其他的数值都会返回`true`。



判断是否为整数：**isInteger**()

指定四舍五入的位数：**toFixed**(位数)

使用 **Object.is** 方法判断两个值是否完全相同

```js
console.log(Object.is(+0, -0));//false
console.log(Object.is(+0, 0));//true
console.log(Object.is(NaN, NaN));//true
```







## Math 对象

### 静态属性

`Math`对象的静态属性，提供以下一些数学常数。

- `Math.E`：常数`e`。
- `Math.LN2`：2 的自然对数。
- `Math.LN10`：10 的自然对数。
- `Math.LOG2E`：以 2 为底的`e`的对数。
- `Math.LOG10E`：以 10 为底的`e`的对数。
- `Math.PI`：常数`π`。
- `Math.SQRT1_2`：0.5 的平方根。
- `Math.SQRT2`：2 的平方根。

这些属性都是只读的，不能修改。





### 静态方法

`Math`对象提供以下一些静态方法。

- `Math.abs()`：绝对值
- `Math.ceil()`：向上取整
- `Math.floor()`：向下取整
- `Math.max()`：最大值
- `Math.min()`：最小值
- `Math.pow()`：幂运算
- `Math.sqrt()`：平方根
- `Math.log()`：自然对数
- `Math.exp()`：`e`的指数
- `Math.round()`：四舍五入
- `Math.random()`：随机数



```javascript
 				//Math.max() 函数返回一组数中的最大值
        var max = Math.max(12, 23, 34, 54, 656, 32, 1, 354);
        console.log(max); //656

        // 返回最小值 Math.min()
        console.log(Math.min(23, 34, 1, 34)); //1


        // 绝对值 Math.abs()
        console.log(Math.abs(-10)); //10

        // 三个取整方法：
        //向上取整 Math.ceil()
        console.log(Math.ceil(1.9)); //2
        console.log(Math.ceil(1.1)); //2

        //向下取整 Math.floor()
        console.log(Math.floor(1.1)); //1
        console.log(Math.floor(1.9)); //1

        //四舍五入 Math.round()
        console.log(Math.round(-1.4));
        //.5比较特殊，四舍五入时往较大值方向取值
        console.log(Math.round(-1.5)); //-1
        console.log(Math.round(-1.9)); //-2
```

```javascript
        //  Math.random() 函数返回一个浮点数,在0（包括0）和1（不包括）之间
        function getRandom() {
            return Math.random();
        }
        console.log(getRandom());


        //得到两个数之间的随机浮点数，从min到max之间的随机数
        function getRandom1(min, max) {
            return Math.random() * (max - min) + min;
        }
        console.log(getRandom1(0, 10));


        //得到两个数之间的随机 整数, 这个公式取值包括最小值，但不包括最大值
        function getRandom2(min, max) {
            min = Math.ceil(min);
            max = Math.floor(max);
            return Math.floor(Math.random() * (max - min)) + min;
        }
        console.log(getRandom2(0, 10));

        //取两个数之间的随机整数，需要包括这两个值
        function getRandom3(min, max) {
            min = Math.ceil(min);
            max = Math.floor(max);
            return Math.floor(Math.random() * (max - min + 1)) + min;
        }
        console.log(getRandom3(0, 10));


        //案例：随机点名
        var arry = ['张一百', '王有才', '李四海', '潘虹', '柳源'];
        console.log(arry[getRandom3(0, arry.length - 1)]);
```

使用**apply**从数组中取值：

```javascript
Math.max.apply(Math, [1, 2, 3]);
```



### 三角函数方法

`Math`对象还提供一系列三角函数方法。

- `Math.sin()`：返回参数的正弦（参数为弧度值）
- `Math.cos()`：返回参数的余弦（参数为弧度值）
- `Math.tan()`：返回参数的正切（参数为弧度值）
- `Math.asin()`：返回参数的反正弦（返回值为弧度值）
- `Math.acos()`：返回参数的反余弦（返回值为弧度值）
- `Math.atan()`：返回参数的反正切（返回值为弧度值）







## Date对象

### Date()

直接调用Date()，返回一个代表当前时间的字符串。

```js
      console.log(Date()); //返回当前时间
      //Thu Jun 03 2021 16:09:08 GMT+0800 (中国标准时间)
      //参数是没有作用的
      console.log(Date(1990)); //返回的仍是当前时间
      //Thu Jun 03 2021 16:09:08 GMT+0800 (中国标准时间)
```





### new Date()

返回一个Date对象的实例。

如果不加参数，实例代表的就是当前的时间。

```js
      //创建Date 实例
      let today = new Date();
      console.log(today); //当前时间
      //Thu Jun 03 2021 16:09:08 GMT+0800 (中国标准时间)

      //如果加参数
      let bornDay = new Date("2018-08-24 17:45");
      console.log(bornDay);
      //Fri Aug 24 2018 17:45:00 GMT+0800 (中国标准时间)
```







### Date.now() 获取当前时间戳

```javascript
console.log(Date.now());//1622708280624
```



案例： 计算脚本执行时间

```javascript
const start = Date.now();
for(let i = 0; i< 1000000; i++){}//模拟代码执行
const end = Date.now()
console.log(end - start);
```





### Date.parse()

用来解析日期字符串，返回该时间的时间戳

```js
      let bornDay = new Date("2018-08-24 17:45");
      console.log(bornDay);
      //Fri Aug 24 2018 17:45:00 GMT+0800 (中国标准时间)
      //Date.parse() 解析字符串参数，返回该时间点的时间戳
      console.log(Date.parse(bornDay)); //1535103900000
      console.log(Date.parse("2018-08-24 17:45")); //1535103900000

      //当前时间戳
      console.log(Date.now()); //1622708280624

      //如果解析失败，返回NaN
      console.log(Date.parse("xxx")); // NaN
```









### 实例方法

`Date`的实例对象，有几十个自己的方法，除了`valueOf`和`toString`，可以分为以下三类。

- `to`类：从`Date`对象返回一个字符串，表示指定的时间。
- `get`类：获取`Date`对象的日期和时间。
- `set`类：设置`Date`对象的日期和时间。













### 格式化日期

```javascript
        //返回时间格式为 15:02:30
        function getTime() {
            var time = new Date();
            var hour = time.getHours();
            var minutes = time.getMinutes();
            var seconds = time.getSeconds();

            //用三元表达式 更简洁
            //条件表达式 ？ 表达式1 ： 表达式2
            hour = hour < 10 ? '0' + hour : hour;
            minutes = minutes < 10 ? '0' + minutes : minutes;
            seconds = seconds < 10 ? '0' + seconds : seconds;
            return hour + ':' + minutes + ':' + seconds;
        }
        console.log(getTime());
```

格式化日期2:

```javascript
function dateFormat(date, format = "YYYY-MM-DD HH:mm:ss") {
  const config = {
    YYYY: date.getFullYear(),
    MM: date.getMonth() + 1,
    DD: date.getDate(),
    HH: date.getHours(),
    mm: date.getMinutes(),
    ss: date.getSeconds()
  };
  for (const key in config) {
    format = format.replace(key, config[key]);
  }
  return format;
}
console.log(dateFormat(new Date(), "YYYY年MM月DD日"));

```



#### moment.js

Moment.js是一个轻量级的JavaScript时间库，它方便了日常开发中对时间的操作，提高了开发效率。

http://momentjs.cn/



```javascript
//获取当前时间

console.log(moment().format("YYYY-MM-DD HH:mm:ss"));

```







## RegExp 对象

RegExp 对象提供正则表达式的功能。

### 概述

正则表达式，regular rexpression，是一种表达文本模式的方法。



**新建正则表达式：**

- 使用字面量：
  - `var reg = /xyz/i;`
- 使用RegExp构造函数
  - `var reg = new RegExp('xyz')`





### 实例属性

正则对象的实例属性分成两类。

一类是**修饰符相关**，用于了解**设置了什么修饰符**：

- `RegExp.prototype.ignoreCase`：返回一个布尔值，表示是否设置了`i`修饰符。
- `RegExp.prototype.global`：返回一个布尔值，表示是否设置了`g`修饰符。
- `RegExp.prototype.multiline`：返回一个布尔值，表示是否设置了`m`修饰符。
- `RegExp.prototype.flags`：返回一个字符串，包含了已经设置的所有修饰符，按字母排序。

上面四个属性都是只读的。

另一类是与修饰符**无关**的属性，主要是下面两个：

- `RegExp.prototype.lastIndex`：返回一个整数，表示下一次开始搜索的位置。该属性可读写，但是只在进行连续搜索时有意义，详细介绍请看后文。
- `RegExp.prototype.source`：返回正则表达式的字符串形式（不包括反斜杠），该属性只读。





### 实例方法

#### RegExp.prototype.test()

正则实例对象的test方法返回一个布尔值，表示当前模式是否匹配到参数字符串。

```js
      //test()
      let animals = "cats and dogs";
      console.log(/cat/.test("cats and dogs")); //true
      console.log(/dog/.test(animals)); //true

      let r = /ca/g; //g是修饰符，代表全局匹配
      //lastIndex
      console.log(r.lastIndex); //0 lastIndex属性默认为0，代表开始搜索的位置
      //每一次开始搜索的位置都是上一次匹配的后一个位置
      console.log(r.test(animals)); //true
      console.log(r.lastIndex); //2
      console.log(r.test(animals)); //false
      console.log(r.lastIndex); //0 搜索完毕，又从头开始
      console.log(r.test(animals)); //true

      let r1 = /a/g;
      letters = "abcdefg";
      r1.lastIndex = 2;
      console.log(r1.test(letters)); //false 从第3个值开始搜，并没有'a'
```



#### RegExp.prototype.exec()

exec() 方法用来返回匹配结果。如果发现匹配，就返回一个数组，成员是匹配成功的子字符串，否则就返回null.

```js
      //exec()
      //如果匹配到结果，就返回一个数组，包含匹配成功的字符串
      //否则，返回null

      let baby = "wangyuce";
      let r1 = /a/;
      let r2 = /b/;
      console.log(r1.exec(baby)); //["a", index: 1, input: "wangyuce", groups: undefined]
      console.log(r2.exec(baby)); //null

      //组匹配()
      let r3 = /_(x)/;
      console.log(r3.exec("_x_x"));//["_x", "x", index: 0, input: "_x_x", groups: undefined]
      //结果数组里的第2个成员是圆括号里匹配到的结果
      //input，整个原字符串
      //index，匹配成功的开始位置
```





### 与正则相关的字符串实例方法

字符串的实例方法之中，有4种与正则表达式有关：

- `String.prototype.match()`：返回一个数组，成员是所有匹配的子字符串。
- `String.prototype.search()`：按照给定的正则表达式进行搜索，返回一个整数，表示匹配开始的位置。
- `String.prototype.replace()`：按照给定的正则表达式进行替换，返回替换后的字符串。
- `String.prototype.split()`：按照给定规则进行字符串分割，返回一个数组，包含分割后的各个成员。



#### match()

字符串实例对象的`match`方法对字符串进行正则匹配，返回匹配结果。

```js
      let baby = "wangyuce";
      //   let r1 = /a/;
      //   let r2 = /b/;
      //match()
      //返回匹配到的结果,与exec()结果类似
      console.log(baby.match(/a/)); //["a", index: 1, input: "wangyuce", groups: undefined]
      console.log(baby.match(/b/)); //null

      //match与exec区别在于：
      //1. 使用修饰符g进行全局匹配时，会一次性返回所有匹配到的结果
      //2. 设置lastIndex无效，匹配总是从头部开始
      let mom = "chenhongfei";
      console.log(mom.match(/n/g)); // ["n", "n"]
      console.log(/n/g.exec(mom)); //["n", index: 3, input: "chenhongfei", groups: undefined]
```









#### search()

字符串对象的`search`方法，返回**第一个**满足条件的匹配结果在整个字符串中的位置。如果没有任何匹配，则返回`-1`。

```js
      //search()返回第一个匹配到的位置
      let song = "apple red, apple round";
      let r1 = /apple/;
      console.log(song.search(r1)); //0
      console.log(song.search(/pear/));//-1 没有匹配到就返回-1
```











#### replace()

字符串对象的`replace`方法可以替换匹配的值。它接受两个参数，第一个是正则表达式，表示搜索模式，第二个是替换的内容。

```js
      let song = "apple red, apple round";

      //replace()替换
      console.log(song.replace(/apple/, "pear")); //"pear red, apple round" 没有加修饰符g，只替换了一次
      console.log(song.replace(/apple/g, "pear")); //"pear red, pear round" 加了g，全部替换
```









#### split()











### 匹配规则

#### 字面量字符和元字符

**字面量字符**

在正则表达式之中，某个字符只表示它字面的含义（就像前面的`a`和`b`），那么它们就叫做“字面量字符”（literal characters）。

比如`/a/`匹配`a`，`/b/`匹配`b`。



**元字符**

有特殊含义，不代表字面的意思：

- **点字符 .**  
  - `/c.t/` 匹配 c 和 t 之间包含意义一个字符的情况，比如 cat, c2t, cxt 等，中间只能有一个其他字符，不能是caat
- **位置字符**，用来提示字符所处的位置
  - `^`  表示字符串的开始位置
  - `$`  表示字符串的结束位置
- **选择符**
  - `|`  竖线符号，表示 “或关系”，or， `cat|dog`表示匹配 cat或者dog

```js
      //^ $
      //^ 开始位置
      //$ 结束位置
      let song = "apple red, apple round, apple juicy, apple sweet";
      console.log(/^apple/.test(song)); //true 开始位置有apple吗？
      console.log(/$apple/.test(song)); //false 结束位置有apple吗？
      //^ $ 结合使用，表示从开始到结束只有apple吗？
      console.log(/^apple$/.test(song)); // false
      console.log(/^apple$/.test("apple")); //true

      //选择符 ｜ 或的意思
      console.log(/12|22/g.test("12,13")); //true
      console.log(/11|22|33/.test("2131123")); //true
```



- 其他
  - `\`  转义符
  - `*`
  - `+`
  - `?`
  - `()`
  - `[]`
  - `{}`



#### 转义符

匹配那些本身有特殊含义的元字符，就需要在前面加上反斜杠转义符`\`。

正则表达式中，需要反斜杠转义的，一共有12个字符：`^`、`.`、`[`、`$`、`(`、`)`、`|`、`*`、`+`、`?`、`{`和`\`。需要特别注意的是，如果使用`RegExp`方法生成正则对象，转义需要使用两个斜杠，因为字符串内部会先转义一次。





#### 特殊字符

正则表达式对一些不能打印的特殊字符，提供了表达方法。

- `\cX` 表示`Ctrl-[X]`，其中的`X`是A-Z之中任一个英文字母，用来匹配控制字符。
- `[\b]` 匹配退格键(U+0008)，不要与`\b`混淆。
- `\n` 匹配换行键。
- `\r` 匹配回车键。
- `\t` 匹配制表符 tab（U+0009）。
- `\v` 匹配垂直制表符（U+000B）。
- `\f` 匹配换页符（U+000C）。
- `\0` 匹配`null`字符（U+0000）。
- `\xhh` 匹配一个以两位十六进制数（`\x00`-`\xFF`）表示的字符。
- `\uhhhh` 匹配一个以四位十六进制数（`\u0000`-`\uFFFF`）表示的 Unicode 字符。



#### 字符类

`[]`，字符类，class，表示有一系列字符可供选择，只要匹配其中一个就可以

**脱字符**`^`

```js
      //[]字符类，只要匹配其中一个就可以
      console.log(/[abc]/.test("dfhkf")); //false
      console.log(/[abc]/.test("dfhakf")); //true
      //[^xxx] 脱字符 除去这些都可以 ^放在开头才有脱字符的作用，否则就是开始位置
      console.log(/[^abc]/.test("dfhkf")); //true 表示除了abc都可以,
      console.log(/[^abc]/.test("dfhakf")); //true 只要包含了 abc之外的 都是true
      console.log(/[^abc]/.test("aabbcccc")); //false 只有abc 没有别的 false
      //[^] 如果只是这样 代表可以匹配一切字符，包括换行符
      console.log(/[^]/.test("\n")); //true
```



**连字符 **`-`

可表示字符的连续范围

比如，`[abc]`可以写成`[a-c]`，`[0123456789]`可以写成`[0-9]`，同理`[A-Z]`表示26个大写字母。

```js
      //连字符 - [-] 一定要结合[]使用
      //当连字号（dash）不出现在方括号之中，就不具备简写的作用
      //只有当连字号用在方括号之中，才表示连续的字符序列。
      //表示范围
      console.log(/a-c/.test("apple")); //false
      console.log(/[a-c]/.test("abcdef")); //true  []表示其中一个符合就可以 [a-z]表示a-z其中一个符合就可以
      console.log(/[a-c]/.test("error")); //false
      //[1-31] 表示的是1-3的范围
```







#### 预定义模式

预定义模式指的是某些**常见模式**的**简写**方式。

- `\d` 匹配0-9之间的任一数字，相当于`[0-9]`。（**数字字符**）
- `\D` 匹配所有0-9以外的字符，相当于`[^0-9]`。（**非数字字符**）
- `\w` 匹配**任意的字母、数字和下划线**，相当于`[A-Za-z0-9_]`。
- `\W` 除所有字母、数字和下划线以外的字符，相当于`[^A-Za-z0-9_]`。
- `\s` 匹配空格（包括换行符、制表符、空格符等），相等于`[ \t\r\n\v\f]`。（**空白符**）
- `\S` 匹配非空格的字符，相当于`[^ \t\r\n\v\f]`。（**非空白符**）
- `\b` 匹配词的边界。
- `\B` 匹配非词边界，即在词的内部。

```js
      //\d 数字字符
      //\D 非数字字符
      //\w 任意的字母、数字、下划线
      //\W 除去字母、数字、下划线
      //\s 空白符
      //\S 非空白符
      //\b 单词边界？
      //\B 词的内部

      //\d 数字字符，只要有数字就是true，没有一个数字就是false
      console.log(/\d/.test("afad")); //false 没有数字
      console.log(/\d/.test("afa11d")); //true
      //--------------
      //\D 非数字字符 只要包含0-9之外的字符就是true，也就是说只有纯数字的情况才为false
      console.log(/\D/.test("afad")); //true
      console.log(/\D/.test("afa11d")); //true
      console.log(/\D/.test("123")); //false 只有数字，没有匹配到非数字
      //--------------
      //\w 任意的字母、数字、下划线
      console.log(/\w/.test("afad")); //true
      console.log(/\w/.test("\n\t")); //false
      console.log(/\w/.test("___")); //true
      console.log(/\w/.test("2314")); //true
      //--------------
      //\W 除去字母、数字、下划线
      console.log(/\W/.test("adfa")); //false
      console.log(/\W/.test("\n\t   ")); //true
      console.log(/\W/.test("213")); //false
      //--------------
      //\s 空白符
      console.log(/\s/.test("  ")); //true
      console.log(/\s/.test("abc")); //false
      console.log(/\s/.test(" \t add ")); //true 能匹配到空白符就是true
      //--------------
      //\S 非空白符
      console.log(/\S/.test("   ")); //false 有空白就是false
      console.log(/\S/.test("aadg")); //true
      //--------------
      //\b 匹配词边界
      //\b 放在单词前面还是后面是有区别的，放在哪里就表示单词的哪边不能紧跟着字符必须独立
      console.log(/\bfei/.test("hong fei")); //true
      console.log(/\bfei/.test("hongfei")); //false
      console.log(/fei\b/.test("hong fei")); //true
      console.log(/fei\b/.test("hongfei")); //true
      console.log(/fei\b/.test("hongfeichen")); //false 这里要匹配fei的后面要独立 所以false
      console.log(/fei\b/.test("hongfei chen")); //true

      //--------------
      //\B 匹配词的内部
      //\B 放在哪里就表示词的哪边不能独立
      console.log(/\Bfei/.test("hong fei")); //false fei的前面不能独立所以false
      console.log(/\Bfei/.test("hongfei")); //true
      console.log(/fei\B/.test("hong fei")); //false fei的后面不能独立所以false
      console.log(/fei\B/.test("hongfei")); //false
      console.log(/fei\B/.test("hongfeichen")); //true
      console.log(/fei\B/.test("hongfei chen")); //false
      //--------------
      //[\S\s]能指代一切字符。
      let html = "<b>Hello</b>\n<i>world!</i>";
      /[\S\s]*/.exec(html)[0];
      // "<b>Hello</b>\n<i>world!</i>"
```





#### 重复类

如果要精确的匹配**字符重复**的**次数**，用`{}`。

`{n}`表示恰好重复`n`次，`{n,}`表示至少重复`n`次，`{n,m}`表示重复不少于`n`次，不多于`m`次。

```js
      //{} 检测字符重复的次数
      //{n} 恰好重复n次
      //{n,} 至少重复n次
      //{n,m} 重复的次数大于等于n，小于等于m次

      //-----------
      //{n} 恰好重复n次
      console.log(/po{3}kie/.test("pooooooookie")); //false  需要匹配正好出现3次o的
      console.log(/po{3}kie/.test("poookie")); //true  需要匹配正好出现3次o的
      console.log(/po{3}kie/.test("poooss")); //false  后面不一致
      //-----------
      //{n,} 至少重复n次
      console.log(/po{3,}kie/.test("pooooooookie")); //true  需要匹配至少出现3次o的
      console.log(/po{3,}kie/.test("pokie")); //false  小于3次
      //-----------
      //{n,m} 重复的次数大于等于n，小于等于m次
      console.log(/po{3,6}kie/.test("pooooooookie")); //false  需要匹配出现3-6次o的
      console.log(/po{3,6}kie/.test("pooookie")); //true  需要匹配出现3-6次o的
```







#### 量词符

量词符用来设定**某个模式**出现的**次数**:

- `?` 问号表示某个模式出现0次或1次，等同于`{0, 1}`。
- `*` 星号表示某个模式出现0次或多次，等同于`{0,}`。
- `+` 加号表示某个模式出现1次或多次，等同于`{1,}`。

```js
      //量词符
      //? 等同于 {0,1} 某个模式出现0次或者1次
      //* 等同于 {0, } 出现0次或多次（在*这个位置上吧）
      //+ 等同于 {1, } 出现1次或多次
      console.log(/t?est/.test("test")); //true
      console.log(/t?est/.test("est")); //true
      console.log(/t?est/.test("ttttest")); //true
      //-----------
      console.log(/t*est/.test("test")); //true
      console.log(/t*est/.test("est")); //true
      console.log(/t*est/.test("ttttest")); //true
      //-----------
      console.log(/t+est/.test("test")); //true
      console.log(/t+est/.test("est")); //false
      console.log(/t+est/.test("ttttest")); //true
```



#### 贪婪模式

**贪婪模式** ：使用量词符时，默认都是最大可能匹配，即匹配到下一个字符不满足规则为止。



**非贪婪模式**：在量词符后面加上一个问号，表示最小可能匹配，只要一匹配到就返回结果，不再继续坚持。如`/t+?/`

- `+?`：表示某个模式出现1次或多次，匹配时采用非贪婪模式。
- `*?`：表示某个模式出现0次或多次，匹配时采用非贪婪模式。
- `??`：表格某个模式出现0次或1次，匹配时采用非贪婪模式。

```js
'abb'.match(/ab*/) // ["abb"]
'abb'.match(/ab*?/) // ["a"]

'abb'.match(/ab?/) // ["ab"]
'abb'.match(/ab??/) // ["a"]
```





#### 修饰符

修饰符（modifier）表示模式的附加规则，放在正则模式的最尾部。

修饰符可以单个使用，也可以多个一起使用。

- **g 修饰符**，全局匹配（global）,正则对象将匹配全部符合条件的结果，主要用于搜索和替换。
- **i 修饰符**，加上`i`修饰符以后表示忽略大小写（ignoreCase）。
- **m 修饰符**，`m`修饰符表示多行模式（multiline），会修改`^`和`$`的行为。









#### 组匹配

`()`，在正则表达式里面使用圆括号，表示分组匹配，括号中的模式可以用来匹配分组的内容。



待补充











## JSON 对象

### JSON 格式

JSON格式（JavaScript Object Notation）是一种用于数据交换的文本格式，目的是取代繁琐笨重的XML。

- 书写简单
- 采用JavaScript原生语法



每个JSON对象就是**一个**值（对象、数组、或者原始类型的值）。





### JSON 对象

JSON 对象是JavaScript 原生的对象，用来处理JSON格式的数据。

它有两个静态方法：`JSON.stringify()`和`JSON.parse()`。



#### JSON.stringify()

`JSON.stringify()`方法用于**将一个值转为 JSON 字符串**。该字符串符合 JSON 格式，并且可以被`JSON.parse()`方法还原。

```js
      let student = {
        name: "John",
        age: 30,
        isAdmin: false,
        courses: ["html", "css", "js"],
        wife: null,
      };

      //使用JSON.stringify()将对象转换为JSON
      let json = JSON.stringify(student);
      console.log(json); //'{"name":"John","age":30,"isAdmin":false,"courses":["html","css","js"],"wife":null}'

      console.log(typeof json); //string
```

得到的 `json` 字符串是一个被称为 **JSON 编码（JSON-encoded）** 或 **序列化（serialized）** 或 **字符串化（stringified）** 或 **编组化（marshalled）** 的对象。我们现在已经准备好通过有线发送它或将其放入普通数据存储。

请注意，JSON 编码的对象与对象字面量有几个重要的区别：

- 字符串使用**双引号**。JSON 中没有单引号或反引号。所以 `'John'` 被转换为 `"John"`。
- 对象属性名称也是双引号的。这是强制性的。所以 `age:30` 被转换成 `"age":30`。

JSON 支持以下数据类型：

- Objects `{ ... }`
- Arrays `[ ... ]`
- Primitives：
  - strings，
  - numbers，
  - boolean values `true/false`，
  - `null`。

JSON 是语言无关的纯数据规范，因此一些特定于 JavaScript 的对象属性会被 `JSON.stringify` 跳过，如：

- 函数属性（方法）。
- Symbol 类型的属性。
- 存储 `undefined` 的属性。



`JSON.stringify`方法会排除`enumerable`为`false`的属性，有时可以利用这一点。如果对象的 JSON 格式输出要排除某些属性，就可以把这些属性的`enumerable`设为`false`。



**toJSON()**

如果一个对象具有 `toJSON`，那么它会被 `JSON.stringify` 调用。





#### JSON.parse()

`JSON.parse()`方法用于将 JSON 字符串转换成对应的值。

语法：

```javascript
let value = JSON.parse(str, [reviver]);
```

JSON 不支持注释。









# 数组

## 定义

数组，array, 是按次序排列的一组值。每个值的位置都有编号（从0开始，index）。

任何类型的数据，都可以放入数组。

```js
var arr = [
  {a: 1},
  [1, 2, 3],
  function() {return true;}
];

arr[0] // Object {a: 1}
arr[1] // [1, 2, 3]
arr[2] // function (){return true;}
```



如果数组的元素还是数组，就形成了**多维数组**。

```js
var a = [[1, 2], [3, 4]];
a[0][1] // 2
a[1][1] // 4
```



>  数组是引用类型，使用const声明时，可以修改里面的值。





## 数组的本质

本质上，数组属于一种特殊的对象，用typeof运算符返回数组的类型是object.

```js
typeof [1, 2, 3] // "object"
```

数组的特殊性体现在，它的**键名**是**按次序排列**的一组整数（0，1，2...）。(也就是说数组的索引其实就是数组对象的键名)

```js
      //数组本质上对象，数组对象的键名就是其索引
      let arr = ["a", "b", "c"];
      console.log(typeof arr); //object
      let keys = Object.keys(arr);
      console.log(keys); //["0", "1", "2"]
			//数组的键名也如对象所规定的，是字符串
```



## in 运算符

检查某个键名是否存在的运算符in，适用于对象，也适用于数组。





## Array.of

`Array.of()` 方法创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。

 **Array.of()** 和 **Array** 构造函数之间的**区别**在于处理整数参数：Array.of(7) 创建一个具有单个元素 7 的数组，而 Array(7) 创建一个长度为7的空数组（注意：这是指一个有7个空位(empty)的数组，而不是由7个undefined组成的数组）。

```javascript
Array.of(7);       // [7]
Array.of(1, 2, 3); // [1, 2, 3]

Array(7);          // [ , , , , , , ]
Array(1, 2, 3);    // [1, 2, 3]
```



## 类型检测

`isArray` 检测变量是否为数组类型

```javascript
let arr = [1, 2];
console.log(Array.isArray(arr)); //true
```



## 类型转换

###  **转为字符串**

toString()

String()

join('') 数组元素连接成为字符串





### 转为数组

#### Array.from

可将类数组、伪数组、可迭代对象转换为数组；类数组是指包含length属性或可迭代对象。

- 第一个参数为要转换的数据，第二个参数为类似于`map` 函数的回调方法

 

```javascript
<body>
    <button message="后盾人">button</button>
    <button message="hdcms">button</button>
</body>

<script>
    let btns = document.querySelectorAll('button');
    console.log(btns); //包含length属性
    Array.from(btns, (item) => {
        item.style.background = 'red';
    });
</script>
```



#### Array.prototype.slice.call

**类似数组的对象**

如果一个对象的所有键名都是正整数或零，并且有length属性，那么这个对象就很像数组，语法称之为“类似数组的对象”。

类数组不具备数组特有的方法。典型的类数组：函数的arguments对象、DOM元素集合、字符串

```js
// arguments对象
function args() { return arguments }
var arrayLike = args('a', 'b');

arrayLike[0] // 'a'
arrayLike.length // 2
arrayLike instanceof Array // false

// DOM元素集
var elts = document.getElementsByTagName('h3');
elts.length // 3
elts instanceof Array // false

// 字符串
'abc'[1] // 'b'
'abc'.length // 3
'abc' instanceof Array // false
```

数组的slice方法可以**将类数组变成真正的数组**：

```js
var arr = Array.prototype.slice.call(arrayLike);
Array.prototype.slice.call(arrayLike, index) //从第index个开始转换成数组
```





#### 使用展开语法转换为数组

[...divs]

```javascript
//使用展开语法将NodeList转换为数组操作
<style>
    .hide {
      display: none;
    }
</style>

<body>
  <div>hdcms</div>
  <div>houdunren</div>
</body>

<script>
  let divs = document.querySelectorAll("div");
  [...divs].map(function(div) {
    div.addEventListener("click", function() {
      this.classList.toggle("hide");
    });
  });
</script>
```



## 展开语法

### 数组合并

```javascript
      const a = [1, 2, 3];
      const b = [4, 5, 6];
      const c = ["Hi", ...a, ...b];
      console.log(c); //["Hi", 1, 2, 3, 4, 5, 6]
```

跟解构数组语法里的剩余模式有点像？？

### 函数参数

使用展开语法可以替代arguments来接收任意数量的参数

```javascript
function fn(...args) {
  console.log(args); //会打印出调用函数时传递的所有的参数
}
fn(1, 2, 3);
```



### 用于转换类型

上例中使用[...divs]将节点列表转换为数组使用数组方法遍历







## 解构赋值

在对象里使用的比在数组里用的多。

**解构赋值语法**是一种JavaScript表达式。通过解构赋值，可以将属性/值从对象/数组中取出，赋值给其他变量。

**Destructuring**



### 数组解构赋值

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



```javascript
let a, b, rest;
[a, b] = [10, 20];
// ... 是剩余模式，将数组剩余部分赋值给一个变量
[a, b, ...rest] = [10, 20, 30, 40, 50, 60];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50, 60] 
```



解构赋值表达式左边定义了要从原变量中取出什么变量。

```javascript
let x = [1, 2, 3, 4, 5];
let [y, z] = x; // 这里解构赋值其实就是把 变量x 的前两个值给 y z
console.log(y); // 1
console.log(z); // 2 
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





### 对象解构赋值

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





## 管理数组元素及数组方法

### 管理元素

**添加元素**

使用索引或length 追加元素

使用扩展语法批量添加元素

arr.push() 在末尾添加元素

arr.unshift() 从前面添加一个元素

```javascript
//使用索引，比较麻烦，还要计算索引位置
let arr = [1, 2];
arr[2] = '3';

//使用length
arr[arr.length] = '4';

//使用push() 配合展开语法
let arrA = [1, 2];
let arrB = [3, 4];
arrA.push(...arrB);//展开语法把所有的arrB的元素添加到arrA的后面
console.log(arrA.push(...arrB));//push()的返回值是增加后的数组的长度length
console.log(arrA); //[1, 2, 3, 4]


```

（数据的增加和删除，就是出栈、入栈？）

**删除元素**

arr.shift() 从开头删除一个元素

arr.pop() 从末尾删除一个元素



**填充元素**

fill()

```javascript
//给数组填充5个hi
console.log(Array(5).fill('hi')); //['hi', 'hi', 'hi', 'hi', 'hi']

//在指定位置填充元素
//在数组[1, 2, 3, 4]中的index 1~3填充'a'
console.log([1, 2, 3, 4].fill('a', 1, 3));//[1, "a", "a", 4]

```



**slice & splice实现数组的增删改查**

slice() 截取元素

- `slice(0, 2)` 截取的开始位置和结束位置
- 原数组不会改变，返回一个**新数组**

它会返回一个新数组，将所有从索引 `start` 到 `end`（**不包括 `end`**）的数组项复制到一个新的数组。`start` 和 `end` 都可以是负数，在这种情况下，从末尾计算索引。





**splice()** 

- `splice(0, 2)` 截取的开始位置和**数量**
- **原数组会改变**，是从原数组里拿出来
- 功能更强大
  - `splice(0, 2, 'a', 'b')` 截取并添加元素
  - `splice(1, 1, 'a')` ，实现元素替换
  - `splice(1, 0, 'a')` 不删除，只追加内容

[arr.splice](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) 方法可以说是处理数组的瑞士军刀。它可以做所有事情：添加，删除和插入元素。

```js
arr.splice(start[, deleteCount, elem1, ..., elemN])
```

它从索引 `start` 开始修改 `arr`：删除 `deleteCount` 个元素并在当前位置插入 `elem1, ..., elemN`。最后返回已**被删除元素**的**数组**。









**清空数组**

- 赋值为空值[]
- **长度length**设置为0 
- arr.splice(0, arr.length)

```javascript
      let arr = [1, 2, 3];
      let b = arr;
      b[0] = "a";
      console.log(arr); //["a", 2, 3]
      console.log(b); //["a", 2, 3]

      //在上面arr的值为["a", 2, 3]
      //然后把地址给了b数组
      //这里arr=[], 相当于又开辟了一块空间给arr，只是里面是空的
      //所以只是改变了arr的指向地址
      arr = [];
      console.log(arr); //[]
      console.log(b); //["a", 2, 3]

      //length
      //这样就是把两个数组变量共同指向的内存空间里的数据清空
      let e = [1, 2];
      let f = e;
      console.log(e, f);
      e.length = 0;
      console.log(e, f); //[]
```



### 数组的合并与拆分

join()   `arr.join('')`  数组元素合并成为字符串

split() `str.split('')` 字符串拆分成数组

concat() `arr.concatt(arr1)` 在数组后面合并新的元素，可以使用...语法更方便

copyWithin() 复制



### 查找元素

IndexOf(要查找的元素) 没有时返回-1

lastIndexOf() 从后面开始查找

includes() 返回布尔值

```javascript
//includes实现
let arr = [1, 2, 3, 4];
function includes( arr, find) {
  for (const value of arr) {
    if(value === find) return true;
  }
    return false;
}
console.log(includes(arr, 2));//true


// 把自己写的方法放在对象原型上，就可以使用.调用方法
Array.prototype.includes = xxx;
```

针对引用类型比较有没有某个值时，比较的是内存地址。

所以使用includes查找引用类型的数据是不成功的，可以**使用find 查找引用类型数据**。

find() 返回要查找的值，没有则返回undefined，找到第一个符合的就返回

findIndex() 返回值的index， 和find用法一样，如果没有则返回-1



```javascript
let blogs = [
  {title: '走进JS', author: 'Fei'},
  {title: '走进CSS', author: 'Fei'},
];

let result = blogs.find(item => return item.title === '走进CSS');

console.log(result);
```



### 数组排序

reverse()

sort()

- 默认安装字符编码顺序排序

- 排序数字

  ```javascript
        //默认按照字符编码排序，达不到数字排序，不过可以实现按字母顺序
  			const arr = [1, 2, 35, 4, 56, 32];
        console.log(arr.sort()); //[1, 2, 32, 35, 4, 56]
  
        //升序
        let up = arr.sort(function (a, b) {
          return a - b;
        });
        console.log(up); //[1, 2, 4, 32, 35, 56]
  
        //降序
        let down = arr.sort((a, b) => b - a);
        console.log(down); //[56, 35, 32, 4, 2, 1]
  ```

  ```javascript
        //案例 排序购物车商品价格
        const products = [
          { name: "iPhone", price: 8000 },
          { name: "iPad", price: 4000 },
          { name: "iMac", price: 16000 },
        ];
        console.table(products);
        let upProduct = products.sort((a, b) => b.price - a.price);
        console.table(upProduct);
  ```

  



### 循环遍历

for 

forEach

for...of

在循环内改变value的值时，要注意：

- 值类型不会改变，它会开辟新的空间
- 而引用类型会被直接改变



iterator迭代器方法

keys() 读取索引

values() 读取值

entries() 读取索引+值 



扩展方法

every() 所有value都返回真才为真

some() 只要有一个返回真就返回真



filter() 过滤符合条件的元素



**map()** 

数组映射 将原来的数组元素修改或者添加等，然后返回到一个**新数组**。这里如果是引用类型，且不需要返回新数组，可以直接修改原来数组。

* **功能**：`map()` 方法创建一个**新数组**，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

* **语法**：`map((item, index) => {})`
  * `item`：遍历的项
  * `index`：该次遍历项的索引

* **返回值**：一个新数组，每个元素都是回调函数的结果。

* **代码**：

```js
[1, 2, 3, 4].map(item => item * 2) // [2, 4, 6, 8]

[{
  name: 'jsliang',
  age: 24,
}, {
  name: '梁峻荣',
  age: 124
}].map((item, index) => {
  return `${index} - ${item.name}`;
}) // ['0 - jsliang', '1 - 梁峻荣']
```



**reduce()**

* **功能**：`reduce()` 方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。

* **语法**：`arr.reduce((prev, next) => { return prev + next }`
  * `prev`：数组前一项的值
  * `next`：数组后一项的值
  * `return`：`return` 出来的值，会被当成下一次的 `prev`

* **返回值**：函数累计处理的结果

* **代码**：

```js
[1, 2, 3, 4].reduce((prev, next) => {
  return prev + next;
}); // 10
['前端', 'pang', 'liang'].reduce((prev, next, index) => {
  return (index = 0 ? '-js' : '') + prev + 'js' + next;
}); // 前端-jspang-jsliang
```

```js
      //使用reduce()方法来统计数组里某个元素出现的次数
      const arr = [1, 1, 2, 3, 3, 3, 3, 444];

      function countItem(array, item) {
        //这里需要两个参数，一个是需要检测的数组，一个是这个数组里需要检测的元素
        return array.reduce((total, value) => {
          //   console.log(value); //这里value就是传入的数组的所有元素
          //   console.log(total);
          total += item === value ? 1 : 0;
          return total;
        }, 0);
      }

      const result = countItem(arr, 3);
      console.log(result); //4


			//利用reduce返回数组元素的最大值也很简单
      function maxItem(arr) {
        return arr.reduce((pre, value) => {
          return pre > value ? pre : value;
        }, 0);
      }

      console.log(maxItem(arr)); //444
```

**数组方法案例**

#### forEach()

遍历数组：

```javascript
//语法
let arr = [1,2,3,4];
let sum =0;
arr.forEach(function(value, index, array) {
  //value 就是每个数组元素
  //index 就是数组元素的索引
  //array 是这个数组
  
  //求和只要简单一句
  sum += value;
});
```





#### map()

返回一个**新数组**，数组中的元素为原始数组元素调用函数处理后的值。

```javascript
array.map(function(currentValue,index,arr), thisValue)
```

案例

```javascript
从接口得到数据 res:
let r = res.map(item => {
    return {
        title: item.name,
        sex: item.sex === 1? '男':item.sex === 0?'女':'保密',
        age: item.age,
        avatar: item.img
    }
})

也可以省略 return：
const users=res.items.map(item => ({
    url: item.html_url,      
    img: item.avatar_url,      
    name: item.login,
    })
);
```



#### filter()

主要用于**筛选数组**，它会返回一个**新数组**，里面存放符合指定条件的数组元素。

```javascript
//语法
arr.filter(function(value, index, array){

}, 回调函数)
```

```javascript
 //filter()
            //过滤筛选数组
            let arr = [2, 3, 44, 5, 23, 65, 32];
            // 筛选出大于20的数组元素
            let bigArr = arr.filter((value) => {
                return value >= 20;
            });
            console.log(bigArr); //[44, 23, 65, 32]
```



案例：my-blog中的搜索

```javascript
filterBlogs() {
  return this.blogs.filter((blog)=>{
    return blog.title.match(this.searchText)
  });
}
```

案例：user-admin-demo

```javascript
//筛选用户列表
filterUsers() {
  return this.users.filter((user)=>{
    return user.name.match(this.searchText);
  });
}
```



#### some()

用于检测数组中的元素**是否**满足指定条件，返回**布尔值**

如果找到**第一个满足**条件的元素，则终止循环，不再继续查找

```javascript
arr.some(function(value, index, arr){})
```

```javascript
// 用于查找数组中是否有满足条件的元素，返回布尔值
let arr1 = [2, 3, 5, 1, 3, 4];
//目的：查找数组中是否有大于10的数字？
let newArr1 = arr1.some((value) => {
   return value > 10;
});
console.log(newArr1); //false
```



#### every()

用于检测数组所有元素**是否**符合指定条件，返回**布尔值**。

如果有一个元素**不符合条件**，就会返回false，不再继续检测。

```javascript
array.every(function(currentValue,index,arr), thisValue)
```



案例：综合表单筛选

```javascript
//按价格筛选 价格范围 startPrice~endPrice
filterPrice() {
  return this.price.filter((price)=>{
    return price<=startPrice && price >=endPrice;
  })
}

//按名称搜索 搜索匹配的文本

//按商品名称搜索，商品只有一个的情况，用some最合适，效率更高
findProduct() {
  return this.product.some((product)=> {
    if(product.value === searchText) 
      return true;//停止遍历
  });
}
```



## Array的方法总结

### 静态方法

- Array.isArray()
  - `Array.isArray`方法返回一个布尔值，表示参数是否为数组。它可以弥补`typeof`运算符的不足。



### 实例方法

- valueOf(), toString()
- Push(), pop()
- shift(), unshift()
- join()
- contact()
- reverse()
- slice()
- splice()
- sort()
- map()
- forEach()
- filter()
- some(), every()
- reduce(), reduceRight()
- indexOf(), lastIndexOf()











# Symbol

## 介绍

Symbol 是一种基本数据类型。`Symbol()` 函数会返回symbol类型的值，该类型具有静态属性和静态方法。？？？what？



Symbol 用于防止属性名冲突而产生的，比如向第三方对象中添加属性时。

**Symbol的值是唯一的，独一无二的**不会重复的。

可以把Symbol理解成 特殊的字符串 永远不会重复的一种数据？？



## 声明的方式

### Symbol

```javascript
//Symbol()
let a = Symbol();
let b = Symbol();
console.log(typeof a);//symbol
console.log(a===b); //false
```

#### Symbol的描述属性

`xxx.description` 返回Symbol的描述

```javascript
let b = Symbol('HelloSymbol');
console.log(b.description); //HelloSymbol
```



### Symbol.for, Symbol.keyFor创建全局共享的Symbol

#### Symbol.for

根据描述获取Symbol，如果存在则不会重新创建，如果不存在则新建一个Symbol。这是与Symbol声明的不同

- 使用Symbol.for会在系统中将Symbol登记
- 使用Symbol则不会登记

```javascript
//Symbol.for
let a = Symbol.for('Hello');
```

##### 使用Symbol.keyFor 获取描述

```javascript
let b = Symbol.for('Chika');
console.log(Symbol.keyFor(b)); //Chika
```





## 案例

Symbol 是独一无二的，所以可以保证对象属性的唯一。

Symbol在对象中的声明和访问：

- 使用 `[]` （变量）形式操作
- 不能使用`.` 来操作

```javascript
//比如在学生管理系统中，会有重名的现象，这时可以用Symbol
let student1 = {
	name: '张龙',
  key: Symbol() //通过Symbol的唯一性来标记学生
};
let student2 = {
  name: '张龙',
  key: Symbol()
};
let grade = {
  [student1.key]: {js: 100, css: 99},
  [student2.key]: {js: 90, css: 90}
};
console.log(grade);



```



## Symbol 在缓存容器中的使用

？？没听懂？

取共享数据？

使用`Symbol` 可以解决在保护数据时由于名称相同造成的耦合覆盖问题。





## 扩展特性和对象属性保护

**对象属性保护**

Symbol **不能使用** for...in 或 for...of 遍历操作，这样并拿不到Symbol 属性的值;

**如果对象不想被遍历，可以使用 Symbol进行保护**





### **`Object.getOwnPropertySymbols`**  **获取属性**

在对象中查找Symbol属性

可以使用`Object.getOwnPropertySymbols` 获取所有Symbol属性：

```javascript

  
```



**`Reflect.ownKeys(obj)`**

获取所有属性包括Symbol







<img src="/Users/neofei/Fei/Frontend/笔记/images/symbol.png" alt="img" style="zoom:100%;" />

# Set

Set是ES6新的数据结构，类似数组，但成员的值是唯一的，没有重复的值。

`Set` 是一个特殊的类型集合 —— “值的集合”（没有键），它的每一个值只能出现一次。

它的主要方法如下：

- `new Set(iterable)` —— 创建一个 `set`，如果提供了一个 `iterable` 对象（通常是数组），将会从数组里面复制值到 `set` 中。
- `set.add(value)` —— 添加一个值，返回 set 本身
- `set.delete(value)` —— 删除值，如果 `value` 在这个方法调用的时候存在则返回 `true` ，否则返回 `false`。
- `set.has(value)` —— 如果 `value` 在 set 中，返回 `true`，否则返回 `false`。
- `set.clear()` —— 清空 set。
- `set.size` —— 返回元素个数。

## 创建Set

```javascript
const set = new Set();

//添加数据
set.add(1);
set.add('a');

//或者在创建时添加数据
const set1 = new Set([1,2,3,4,'a']);
```



## Set类型与Array和Object对比

与数组的区别：

- 不会有重复的值

与对象的区别：

```javascript
      //Set 与Object的区别
      let set1 = new Set();
      set1.add(1);
      set1.add("1");
      console.log(set1); //Set(2) {1, "1"}

      let obj = {};
      obj[1] = "hello";
      obj["1"] = "world";
      console.log(obj); //{1: "world"}
      //在Set里，key的数据类型是严格区分的，字符串'1'和数字1是不一样的
      //在对象里，所有的属性都会转成字符串，所有[1]和['1']属性名最后都会转成字符串属性名
```



## Set元素检测与管理

`set.size`

相当于Array里的length

`set.has()`

`set.delete()`

`set.values()`

`set.clear()`

```javascript
      let set = new Set(["hello", "world", "!"]);
      console.log(set);
      console.log(set.size); //3

      console.log(set.delete("!")); //true
      console.log(set.size); //2
      console.log(set.has("hello")); //true
      console.log(set.clear());
      console.log(set.size);//0
```



## 类型转换

**Set转换成数组**

`Array.from(set)`

`[...set]`

```javascript
 let set = new Set([1, 1, 2, 4, 5, 657, 68, 5, 43, 21]);
      //转换成数组
      //Array.from()
      console.log(Array.from(set));
      //[...set]
      console.log([...set]);

      //去除大于10的元素
      //转成数组方便检测
      let set1 = Array.from(set).filter((value) => value <= 10);
      console.log(set1); //[1, 2, 4, 5]
```



**数组转换成Set**

`Array.from(new Set(array))`

`[...new Set(array)]`

 



## 遍历Set类型

`forEach()`

 `for...of`

`Map` 中用于迭代的方法在 `Set` 中也同样支持：

- `set.keys()` —— 遍历并返回所有的值（returns an iterable object for values），
- `set.values()` —— 与 `set.keys()` 作用相同，这是为了兼容 `Map`，
- `set.entries()` —— 遍历并返回所有的实体（returns an iterable object for entries）`[value, value]`，它的存在也是为了兼容 `Map`。



## Set设置网站关键词案例

```javascript
 let obj = {
        data: new Set(),
        //方法
        keywords(words) {
          this.data.add(words);
        },
        show() {
          const ul = document.querySelector("ul");
          ul.innerHTML = "";
          this.data.forEach((value) => {
            ul.innerHTML += `<li>${value}</li>`;
          });
        },
      };

      const input = document.querySelector("input");
      input.addEventListener("blur", function () {
        obj.keywords(this.value);
        obj.show();
      });
```



## Set 实现并集、交集、差集算法实现

```javascript
   let a = new Set([1, 2, 34, 5, 6, 43]);
      let b = new Set([2, 3, 565, 77, 42, 1]);

      //并集
      //去掉重复的
      console.log(new Set([...a, ...b]));

      //差集
      //a与b不同的元素
      console.log(new Set([...a].filter((value) => !b.has(value))));
      console.log(new Set([...b].filter((value) => !a.has(value))));

      //交集
      //共同有的
      console.log(new Set([...a].filter((item) => b.has(item))));
```



## WeakSet 弱集合

WeakSet的成员必须只能是对象类型的值。

`WeakSet` 对象是一些对象值的集合, 并且其中的每个对象值都只能出现一次。在`WeakSet`的集合中是唯一的。

它和 [`Set`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set) 对象的区别有两点:

- 与`Set`相比，`WeakSet` 只能是**对象的集合**，而不能是任何类型的任意值。
- `WeakSet`是弱引用：集合中对象的引用为弱引用。 如果没有其他的对`WeakSet`中对象的引用，那么这些对象会被当**成垃圾回收掉**。 这也意味着WeakSet中没有存储当前对象的列表。 正因为这样，`WeakSet` 是不可枚举的。



```javascript
var ws = new WeakSet();
var foo = {};
var bar = {};

ws.add(foo);
ws.add(bar);

ws.has(foo);    // true
ws.has(bar);   // true

ws.delete(foo); // 从set中删除 foo 对象
ws.has(foo);    // false, foo 对象已经被删除了
ws.has(bar);    // true, bar 依然存在
```



## 引用类型的垃圾回收原理

JS有自动垃圾回收机制，检测不再继续使用的值，然后释放占用的内存。



## WeakSet 弱集合特性

- 不能用循环、遍历等

- 对象不被引用时，不管WeakSet是否在使用该对象都会被删除。

- 也是因为弱引用，WeakSet 结构没有keys( )，values( )，entries( )等方法和size属性
- 当外部引用删除时，希望自动删除数据的情况可以使用`WeakSet`





 

# Map

以前在对象里，只能把字符串及Symbol作为键名（也就是属性名），即便书写时不加引号，最终属性名也都是作为字符串的。



Map是一个带键的数据项的集合，就像一个 object 一样，它们最大的差别是：

- Map 允许把对象、函数、数字、数组、字符串等作为对象里的属性名(key)

它的方法和属性如下：

- `new Map()` —— 创建 map。
- `map.set(key, value)` —— 根据键存储值。
- `map.get(key)` —— 根据键来返回值，如果 `map` 中不存在对应的 `key`，则返回 `undefined`。
- `map.has(key)` —— 如果 `key` 存在则返回 `true`，否则返回 `false`。
- `map.delete(key)` —— 删除指定键的值。
- `map.clear()` —— 清空 map。
- `map.size` —— 返回当前元素个数。



## 创建Map

```javascript
// new 
let map = new Map([['name', 'Fei'], ['age', 30]]);

//追加数据
map.set('gender', 'male');

//也可以链式追加
map.set(18, 'flower').set(70, 'oldRose');

//{}、[]、函数、number等都可以作为属性名
map.set({}, 'hello');
map.set(function(){}, '函数');
```



## Map的增删改查操作

map.get

map.delete

map.clear() //全清空，不会有返回值

map.has()

```javascript
let obj = {
        name: "Harry",
        age: 2,
      };
      map.set(obj, "baby");

      console.log(map.get(obj)); //baby
      console.log(map.delete(70)); //true
      console.log(map.has(18)); //true
      console.log(map.clear()); //undefined
      console.log(map); //Map(0) {}
```





## Map迭代

如果要在 `map` 里使用循环，可以使用以下三个方法：

- `map.keys()` —— 遍历并返回所有的键（returns an iterable for keys），
- `map.values()` —— 遍历并返回所有的值（returns an iterable for values），
- `map.entries()` —— 遍历并返回所有的实体（returns an iterable for entries）`[key, value]`，`for..of` 在默认情况下使用的就是这个。

迭代的顺序与插入值的顺序相同。与普通的 `Object` 不同，`Map` 保留了此顺序。



还可以使用：

- for...of

- forEach()



```javascript
      let map = new Map();
      map.set(1, "Aa");
      map.set(2, "Bb");
      map.set(3, "Cc");
      console.log(map); //Map(3) {1 => "Aa", 2 => "Bb", 3 => "Cc"}

      //获取所有键
      console.log(map.keys()); //MapIterator {1, 2, 3}
      //获取所有值
      console.log(map.values()); //MapIterator {"Aa", "Bb", "Cc"}
      //获取所有键值对
      console.log(map.entries()); //MapIterator {1 => "Aa", 2 => "Bb", 3 => "Cc"}

      //利用for..of遍历键值对
      for (let [key, value] of map.entries()) {
        console.log(key, value);
        /*  1 "Aa"
            2 "Bb"
            3 "Cc" */
      }

      //forEach遍历
      map.forEach((value, key) => {
        console.log(key, value);
        /*  1 "Aa"
            2 "Bb"
            3 "Cc" */
      });
```





## Map 类型转换

Map转换成数组

[...map] [...map.entries()]

[...map.keys()]

[...map.values()]

转换成数组就可以利用数组的一些方法来处理处理，如filter等

```javascript
      const map = new Map();
      map.set(1, "白日");
      map.set(2, "烟火");
      map.set(3, "平地");
      console.log(map);

      let a = [...map].filter((value) => {
        return value.includes("白日");
      });
      console.log(a);
      console.log(new Map(a));//Map(1) {1 => "白日"}
      console.log(...new Map(a).values());//白日
```



## Object.entries: 从对象创建Map

如果想从一个已存在的普通对象来创建一个 Map， 可以使用 `Object.entries(obj)`, 该方法返回对象的键/值对数组，数组格式完全按照Map所需格式。

```js
let obj = {
  name: 'Fei',
  age: 30,
};

let map = new Map(Object.entries(obj));
//Object.entries 返回键/值对数组：[ ["name","John"], ["age", 30] ]。这就是 Map 所需要的格式。
```



## Object.fromEntries: 从Map创建对象

`Object.fromEntries` 方法的作用是相反的：给定一个具有 `[key, value]` 键值对的数组，它会根据给定数组创建一个对象：

```js
let map = new Map();
map.set('name', 'Neo').set('age', 30);

let obj = Object.fromEntries(map);
```





## Map保存DOM节点

 

```javascript
let map = new Map();
document.querySelector('div').forEach(item=>{
  map.set(item, {
    content: item.getAttribute("name");
  });
});

map.forEach((config, el)=>{
  el.addEventListener('click', ()=>{
    alert(config.content);
  });
});
```



## WeakMap 弱映射

`WeakMap` 和 `Map` 的第一个不同点就是，`WeakMap` 的键必须是对象，不能是原始值。

如果我们在 weakMap 中使用一个对象作为键，并且没有其他对这个对象的引用 —— 该对象将会被从内存（和map）中自动清除。

`WeakMap` 不支持迭代以及 `keys()`，`values()` 和 `entries()` 方法。所以没有办法获取 `WeakMap` 的所有键或值。

`WeakMap` 只有以下的方法：

- `weakMap.get(key)`
- `weakMap.set(key, value)`
- `weakMap.delete(key)`
- `weakMap.has(key)`

为什么会限制使用迭代keys、values等呢？

这些都是由 JavaScript 引擎决定的。JavaScript 引擎可能会选择立即执行内存清理，如果现在正在发生很多删除操作，那么 JavaScript 引擎可能就会选择等一等，稍后再进行内存清理。因此，从技术上讲，`WeakMap` 的当前元素的数量是未知的。JavaScript 引擎可能清理了其中的垃圾，可能没清理，也可能清理了一部分。因此，暂不支持访问 `WeakMap` 的所有键/值的方法。



### WeakMap 应用场景

**额外数据的存储**











# 函数

## 概述

函数是一段可以反复调用的代码块。函数还能接受输入的参数，不同的参数会返回不同的值。



### 函数的声明

JavaScript 里有三种声明函数的方法：

1. 函数声明：用function 命令声明的函数
2. 函数表达式：把函数赋值给变量
3. Function 构造函数 （不直观，几乎无人用）

**函数的重复声明**

如果同一个函数被多次声明，后面声明的会覆盖前面的，而且由于函数提升，前面的声明在任何时候都是无效的。





### 圆括号运算符，return语句

调用函数时，使用圆括号运算符进行调用。

return语句，表示返回，有return就返回该语句作为函数返回值，没有return就不返回值或者返回undefined





### 第一等公民

JavaScript 语言将函数看做一种值，与其他值**地位相同**。凡是可以使用值的地方，就能使用函数。

由于函数与其他数据类型地位平等，所以**在 JavaScript 语言中又称函数为第一等公民。**

>来自知乎  [程墨Morgan](https://www.zhihu.com/people/morgancheng)
>
>如果公民分等级，一等公民什么都可以做，次等公民这不能做那不能做。
>JavaScript的函数也是对象，可以有属性，可以赋值给一个变量，可以放在数组里作为元素，可以作为其他对象的属性，什么都可以做，别的对象能做的它能做，别的对象不能做的它也能做。这不就是一等公民的地位嘛。





### 函数名的提升

JavaScript 引擎将函数名视同变量名，采用function命令声明的函数会像变量声明一样，被提升到代码头部。

```js
f();

function f() {}
```



```js
var f = function () {
  console.log('1');
}

function f() {
  console.log('2');
}

f() // 1
```



### NFE

如果函数是通过函数表达式的形式被声明的（不是在主代码流里），并且附带了名字，那么它被称为命名函数表达式（Named Function Expression）。这个名字可以用于在该函数内部进行自调用，例如递归调用等。













## 函数的属性和方法

在 JavaScript 中，**函数就是对象**。

一个容易理解的方式是把函数想象成可被调用的“行为对象（action object）”。我们不仅可以调用它们，还能把它们当作对象来处理：增/删属性，按引用传递等。



### **name属性**

`sayHi.name`

如果函数自己没有提供，那么在赋值中，会根据上下文来推测一个。



### **length属性**

返回函数形参（入参）的个数

```javascript
function f1(a) {}
function f2(a, b) {}
function many(a, b, ...more) {} //rest 参数不参与计数。

alert(f1.length); // 1
alert(f2.length); // 2
alert(many.length); // 2
```

这种特别的情况就是所谓的 [多态性](https://en.wikipedia.org/wiki/Polymorphism_(computer_science)) —— 根据参数的类型，或者根据在我们的具体情景下的 `length` 来做不同的处理。这种思想在 JavaScript 的库里有应用。



### **自定义属性**

```javascript
function sayHi() {
  alert("Hi");

  // 计算调用次数
  sayHi.counter++;
}
sayHi.counter = 0; // 自定义一个counter属性并初始值

sayHi(); // Hi
sayHi(); // Hi

alert( `Called ${sayHi.counter} times` );//2
```

属性不是变量。我们可以把函数当作对象，在它里面存储属性，但是这对它的执行没有任何影响。



### toString()

函数的toString() 方法返回一个字符串，内容是函数的源码。

```js
      //函数的toString()方法
      function fn() {
        return 1;
      }
      console.log(fn.toString());
      //   function fn() {
      //     return 1;
      //   }

      console.log(Math.max.toString()); //function max() { [native code] }
```











## 函数参数

函数运行时，有时需要提供外部的数据，不同的外部数据会得到不同的结果，这些外部数据就叫做**参数**。

```js
function square(x) {
  return x * x; //返回x的平方
}
square(2); //4
square(5); //25
//square函数调用时需要提供参数才能得到结果

//函数调用时没有提供参数
console.log(square()); //NaN 

```

```js
//参数的省略
function f(a, b) {
	return a;
}
console.log(f()); //undefined

//没办法只省略靠前面的参数，而保留靠后的参数，可以在要省略的位置传入undefined
function f(a, b) {
  return a;
}

f( , 1) // SyntaxError: Unexpected token ,(…)
f(undefined, 1) // undefined
```



### 参数的传递方式

函数参数如果是**原始类型**的值（数字、字符串、布尔），传递方式就是**传值**传递passes by value，这意味着在函数体内部修改参数值，**不会影响**到函数外部。

```js
      //函数参数的传递方式
      //简单类型：传值
      let a = 1; //变量a是一个原始类型的值，数字型
      function fn1(x) {
        x = 2; //给参数赋值2，这里的值是参数原始值的拷贝，它的修改不会影响到原始值
        return x;
      }
      fn1(a); //将a作为参数传递给函数fn1，将以传值形式传递
      console.log(fn1(a)); //2
      console.log(a); //1 全局变量a的值并不会被改变
```



如果函数参数是**复合类型**的值（数组、对象、其他函数），传递方式是**传址**传递passes by reference。这意味着传入函数的是原始值的地址，在函数内部修改参数，将会**影响**到原始值。

```js
      //引用类型：传址
      let arr = [1, 2];
      function max(array) {
        delete array[0]; //操作参数
        return array;
      }
      console.log(max(arr)); //[empty, 2]
      console.log(arr); //[empty, 2] 原始值被改变
-------------------------------------
      let obj = { name: "Fei" };
      function objFn(o) {
        o.name = "Neo"; //操作参数
      }
      objFn(obj);
      console.log(obj);//{name: "Neo"} 原始值被改变
```

在引用类型作为参数时，如果是以重新复制的方式，也不会影响到原始值。因为参数指向了一个新的地址。

```js
 let arr1 = [1, 2];
      function max1(array) {
        array = [3, 4]; //这里是赋值，会给array重新引用一块存储了新值的内存地址，不会影响到原始值
        return array;
      }
      console.log(max1(arr1)); //[3, 4]
      console.log(arr1); //[1, 2] 不会影响到原始值

      let obj1 = { name: "Fei" };
      function objFn1(o) {
        // o.name = "Neo";
        o = { name: "Neo" }; //这里参数传进来后被替换成另一个值，也就是指向了一个新的地址，不会影响原始值
        return o;
      }
      console.log(objFn1(obj1)); //{name: "Neo"}
      console.log(obj1); //{name: "Fei"} 原始值没有改变
```



### 同名参数

如果函数形参出现同名，则取最后出现的那个值。

```js
function f(a, a) {
  console.log(a);
}

f(1, 2) // 2
f(1) // undefined
```



### arguments 对象

由于JavaScript 允许函数有**不定数目的参数**，所以需要一种机制，可以在函数体内部**读取所有参数**。这就是arguments对象的由来。

一个名为`arguments`的特殊的类数组对象，包函数函数运行时的所有实参，可以按参数索引使用参数。

arguments 只有在**函数体内部**，才可以使用。

```javascript
function fn() {
  console.log(argumetns.length);//获取实参的数量
}

fn(1, 2, 3); //这里传了3个参数，调用后，arguments.length就是3
```



严格模式下，`arguments`对象与函数参数不具有联动关系。也就是说，修改`arguments`对象不会影响到实际的函数参数。



#### **与数组的关系**

`arguments`**对象**包含所有参数，是一个类数组，也是可迭代对象，但是**不能使用数组方法**，不如Rest吸星大法好使（下面有介绍👇）。

如果要让arguments对象使用数组方法，需要将arguments转成真正的数组：

```js
var args = Array.prototype.slice.call(arguments);

//或者
var args = [];
for(let i = 0; i< arguments.length; i++) {
  args.push(arguments[i]);
}
```



#### **callee 属性**（严格模式禁用！）

禁用我学着干嘛🤪

arguments对象带有一个callee属性，返回它所对应的原函数。

可以通过arguments.callee，达到调用函数自身的目的。

```js
var f = function () {
  console.log(arguments.callee === f);
}

f() // true
```



#### **箭头函数里没有arguments对象**

**箭头函数没有arguments**对象，在箭头函数里面使用arguments拿到的是外层函数的arguments。





### Rest 和 Spread  吸星大法和打散

#### Rest参数 ...

在函数参数里使用 `...args` 将剩余参数收集到一个**数组**中。

```js
function sum(...args){
  let sum = 0;
  for(let arg of args) sum += arg;
  return sum;
}
sum(1, 2, 3); //6
```

我们也可以选择获取第一个参数作为变量，并将剩余的参数收集起来。`function sum(num1, num2, ...args){}`;   **Rest 参数必须放到参数列表的末尾**



#### Spread 语法 ...

**Spread 就是打散，Rest 就是 吸星。** 哈哈

当在函数中使用`...arr`，它会把可迭代对象 arr **展开**到参数列表总。

...Spread语法把数组转换为参数列表

```js
let arr = [1, 2, 3, 4];
//如果想使用Math.max(), 必须把arr里的数值一个个放进方法里
//使用...arr可以直接将元素打散, ...Spread语法把数组转换为参数列表
Math.max(...arr); //4
```

还可以用spread语法合并数组：

```js
let arr1 = [1, 2];
let arr2 = [3, 4];
let mergedArr = [...arr1, ...arr2, 5, 6]; //[1, 2, 3, 4, 5, 6]
```

spread语法内部使用了迭代器来收集元素，与for...of的方式相同。

任何可迭代对象都可以使用spread语法，如果 str, arr等。

```js
//将str 转换为 字符数组
let str = 'APPLE';
console.log([..str]); //['A', 'P', 'P', 'L', 'E]
```



**Array.from(obj) 与 [...obj]**

 `Array.from(obj)` 和 `[...obj]` 存在一个细微的差别：

- `Array.from` 适用于类数组对象也适用于可迭代对象。
- Spread 语法只适用于可迭代对象。

因此，对于将一些“东西”转换为数组的任务，`Array.from` 往往更通用。



**spread 也可以进行浅拷贝**

```js
let arr = [1, 2, 3];
let arrCopy = [...arr]; // 将数组 spread 到参数列表中
                        // 然后将结果放到一个新数组

// 两个数组中的内容相同吗？
alert(JSON.stringify(arr) === JSON.stringify(arrCopy)); // true

// 两个数组相等吗？
alert(arr === arrCopy); // false（它们的引用是不同的）

// 修改我们初始的数组不会修改副本：
arr.push(4);
alert(arr); // 1, 2, 3, 4
alert(arrCopy); // 1, 2, 3
```

```js
let obj = { a: 1, b: 2, c: 3 };
let objCopy = { ...obj }; // 将对象 spread 到参数列表中
                          // 然后将结果返回到一个新对象

// 两个对象中的内容相同吗？
alert(JSON.stringify(obj) === JSON.stringify(objCopy)); // true

// 两个对象相等吗？
alert(obj === objCopy); // false (not same reference)

// 修改我们初始的对象不会修改副本：
obj.d = 4;
alert(JSON.stringify(obj)); // {"a":1,"b":2,"c":3,"d":4}
alert(JSON.stringify(objCopy)); // {"a":1,"b":2,"c":3}
```

这种方式比使用 `let arrCopy = Object.assign([], arr);` 来复制数组，或使用 `let objCopy = Object.assign({}, obj);` 来复制对象写起来要短得多。因此，只要情况允许，我们更喜欢使用它。



**总结**

当我们在代码中看到 `"..."` 时，它要么是 rest 参数，要么就是 spread 语法。

有一个简单的方法可以区分它们：

- 若 `...` 出现在函数参数列表的最后，那么它就是 rest 参数，它会把参数列表中剩余的参数收集到一个数组中。
- 若 `...` 出现在函数调用或类似的表达式中，那它就是 spread 语法，它会把一个数组展开为列表。

使用场景：

- Rest 参数用于创建可接受任意数量参数的函数。
- Spread 语法用于将数组传递给通常需要含有许多参数的列表的函数。

它们俩的出现帮助我们轻松地在列表和参数数组之间来回转换。





### 函数参数默认值 ES6新增

#### 介绍

ES6允许为函数的参数设置默认值，即直接写在参数定义的后面

```js
      //之前为函数设置默认参数：
      function log(x, y) {
        //使用短路运算
        // y = y || "world"; //用短路运算，但是当y是''或者其他对应布尔值为false时，不能正确输出

        //或者使用三元表达式
        y = y === undefined ? "world" : y;

        //或者直接进行y的类型判断
        // if (typeof y === "undefined") y = "world";

        console.log(x, y);
      }
      log("Hello", ""); //Hello
      log("Hello"); //Hello world
--------------------------------------
      //ES6直接设置默认值,简介而自然
      function f1(x, y = "world") {
        console.log(x, y);
      }
      f1("Hello"); //Hello world
      f1("Hello", "China");//Hello China
```

使用默认参数时，函数不能有同名参数。



参数默认值不是传值的，而是每次都重新计算默认值表达式的值（参数默认值是惰性求值）

```js
      //参数默认值是表达式，惰性求值，在取用时才求值
      function f2(p = x + 1) {
        console.log(p);
      }
      let x = 10;
      f2(); //11

      x = 20;
      f2(); //21
//每次调用函数foo，都会重新计算x + 1，而不是默认p等于 100。
```



>惰性求值：表达式不在它被赋值给变量后就立马求值，而是在该值被**取用的时候求值**。



#### 与解构赋值配合使用

```js
      //与解构赋值配合使用
      //只用解构设置默认值，而没有设置参数默认值
      function f3({ a, b = 5 }) {
        console.log(a, b);
      }
      //f3();//报错 如果参数不是对象就会报错
      f3({ a: "Neo", b: 10 }); //Neo 10  如果参数是对象，变量a,b才会解构生成

      //通过设置参数默认值来解决
      function f4({ a, b = 5 } = {}) {
        //参数默认值设置为一个空对象{}
        console.log(a, b);
      }
      f4(); //undefined 5  不会报错了
      f4({ a: "Neo", b: 10 }); //Neo 10
```

```js
      //案例2:
      function fetch(url, { body = "", method = "GET", headers = {} }) {
        console.log(method);
      }

      fetch("http://example.com", {}); // "GET"

      fetch("http://example.com"); // 报错 没有提供第二个参数

      function fetch(url, { body = "", method = "GET", headers = {} } = {}) {
        //设置第二个参数默认值为空对象{}
        console.log(method);
      }

      fetch("http://example.com", {}); // "GET"

      fetch("http://example.com"); // "GET"

//函数fetch没有第二个实参，参数默认值就会生效，然后解构赋值的默认值生效，变量method才会渠道默认值GET
```





#### 参数默认值的位置











## 函数作用域

### 作用域

作用域scope，指的是变量存在的范围。

- 全局作用域 （变量在整个程序中一直存在，所有地方都可以读取）
- 函数作用域 （只在函数内部存在）
- 块级作用域 （新增）

```js
//对于顶层函数来说，函数外部声明的变量就是全局变量，global vairable, 它在函数内部也可以读取
let v = 1;
function f() {
  console.log(v); 
}
f(); //1

//在函数内部定义的变量，外部无法读取，称为“局部变量”，local variable
function f() {
  var v = 1; //v在函数内部定义，是一个局部变量，函数之外无法读取
}
console.log(v); //错误

//函数内部定义的变量，会在该作用域内覆盖同名全局变量
let v = 1;
function f() {
  let v = 2; //函数内又定义了一个变量v，会覆盖掉全局变量v
  console.log(v);
}
f(); //2

//另外再次强调，var声明的变量没有块作用域，它在块内声明的变量依然是全局变量
```



### 函数内部的变量提升

函数内部的变量也会提升。var声明的变量会被提升到函数体的头部。

```js
      //函数内部的变量提升
      function fn() {
        console.log(a);
        var a = 3;
      }
      fn(); //undefined var声明的变量a被提升到函数体的头部
```



### 函数本身的作用域

函数本身也是一个值，也有自己的作用域。他的作用域与变量一样，就是其**声明时所在的作用域**，而不是调用时所在的作用域。

```js
      //函数本身的作用域
      var a = 1; //全局变量a
      var x = function () {
        console.log(a);
      };
      x(); //1
      //函数x 是在全局作用域声明的

      function fn() {
        var a = 2;
        x(); //在函数fn() 内部调用函数 x
      }
      fn(); //1 这里结果是1， 而不是2！
      //因为函数x的作用域是在fn外部，它不会在fn内部取值a


      //同样，函数体内部声明的函数，作用域绑定函数体内部。
      function foo() {
        var x = 1;
        function bar() { //函数bar声明的位置
          console.log(x);
        }
        return bar;
      }

      var x = 2; //不会调用这个外部的x
      var f = foo();
      f(); // 1
```













## 闭包

### 闭包

理解**闭包**，首先要理解变量作用域。

正常情况下，函数外部无法读取函数内部声明的变量；但是出于需要，我们需要得到函数内的局部变量，只有通过变通才能实现：那就是**在函数的内部，再定义一个函数**。

由于JavaScript语言特有的**链式作用域**（是指词法环境吧）结构，子对象会一级一级地向上寻找所有父对象的变量，所以父对象的所有变量，对子对象都是可见的，反之则不成立。

```js
      //在函数内部再声明一个函数
      //比如我们想拿到函数f1内部的变量a，直接在外部取值是不行的
      //在f1内部定义一个函数f2，f2是可以拿到外部的f1中的a，那么这时将f2作为f1的返回值，就可以在外部拿到a了
      //这里f2 就称为闭包
      function f1() {
        let a = 1;
        let b = 2;
        function f2() {
          console.log(a, b); //f2 是可以读取到f1的变量a
        }
        return f2;
      }

      let result = f1();
      result(); //1 2 拿到了函数内部的a和b的值

//闭包就是f2函数，即能够读取其他函数内部变量的函数。
//闭包最大的特点，就是它可以“记住”诞生的环境，比如f1记住了它诞生的环境f1，所以f2可以得到f1的内部变量。
在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。
```

**闭包**，closure, **是**有权访问另一个函数作用域中变量的**函数**。（可以简单理解为 **定义在一个函数内部的函数**）

>  scope，作用域，chrome打断点观察scope

闭包的主要作用：

- 可以读取外层函数内部的变量（延伸了变量的作用范围）

- 让这些变量始终保持在内存中（闭包可以使得它诞生的函数环境一直存在）

- 封装对象的私有属性和私有方法

  - ```js
    function Person(name) {
      var _age;
      function setAge(n) {
        _age = n;
      }
      function getAge() {
        return _age;
      }
    
      return {
        name: name,
        getAge: getAge,
        setAge: setAge
      };
    }
    
    var p1 = Person('张三');
    p1.setAge(25);
    p1.getAge() // 25
    
    //上面代码中，函数Person的内部变量_age，通过闭包getAge和setAge，变成了返回对象p1的私有变量。
    ```

    

**不能滥用闭包：**

外层函数每次运行，都会生成一个新的闭包，而这个闭包又会保留外层函数的内部变量，所以内存消耗很大。滥用闭包，会造成网页的性能问题。



#### 全局对象

- 全局对象包含应该在任何位置都可见的变量。

  其中包括 JavaScript 的内建方法，例如 “Array” 和环境特定（environment-specific）的值，例如 `window.innerHeight` — 浏览器中的窗口高度。

- 全局对象有一个通用名称 `globalThis`。

  ……但是更常见的是使用“老式”的环境特定（environment-specific）的名字，例如 `window`（浏览器）和 `global`（Node.js）。

- 仅当值对于我们的项目而言确实是全局的时，才应将其存储在全局对象中。并保持其数量最少。

- 在浏览器中，除非我们使用 [modules](https://zh.javascript.info/modules)，否则使用 `var` 声明的全局函数和变量会成为全局对象的属性。（容易造成全局对象污染）

- 为了使我们的代码面向未来并更易于理解，我们应该使用直接的方式访问全局对象的属性，如 `window.x`。



#### 词法环境 Lexical Environment

**变量**

JS中，每个运行的函数、代码块、及整个脚本，都有一个 **词法环境** 的内部（隐藏）的关联对象。

词法环境 对象的组成：

- **环境记录**， Environment Record, 一个存储**所有局部变量**作为其**属性**的对象。
- **对外部词法环境的引用**，与外部代码相关联



一个“变量”只是 **环境记录** 这个特殊的内部对象的一个属性。“获取或修改变量”意味着“获取或修改词法环境的一个属性”。

- 变量是特殊内部对象的属性，与当前正在执行的（代码）块/函数/脚本有关。

- 操作变量实际上是操作该对象的属性。



**函数声明**

一个函数其实也是一个值，就像变量一样。

**不同之处在于函数声明的初始化会被立即完成。**



在一个函数运行时，在**调用**刚开始时，会自动创建一个**新的词法环境**以存储这个调用的局部变量和参数。



**当代码要访问一个变量时 —— 首先会搜索内部词法环境，然后搜索外部环境，然后搜索更外部的环境，以此类推，直到全局词法环境。**

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210521143546673.png" alt="image-20210521143546673" style="zoom:50%;" />



**在变量所在的词法环境中更新变量**

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210521143614735.png" alt="image-20210521143614735" style="zoom:50%;" />





#### 闭包

[闭包](https://en.wikipedia.org/wiki/Closure_(computer_programming)) 是指**内部函数总是可以访问其所在的外部函数中声明的变量和参数**，即使在 其外部函数被返回（寿命终结）了之后。

在 JavaScript 中，所有函数都是**天生闭包**的（只有一个例外，将在 ["new Function" 语法](https://zh.javascript.info/new-function) 中讲到）。

JavaScript 中的**函数会自动通过隐藏的 `[[Environment]]` 属性记住创建它们的位置，所以它们都可以访问外部变量**。



**闭包**，closure, **是**有权访问另一个函数作用域中变量的**函数**。

scope，作用域，chrome打断点观察scope

闭包的主要**作用**：

- **延伸了变量的作用范围**

```javascript
function fn() {
  let num = 10;
  
  function fun() {
    console.log(num);
  }
  return fun;
}

let f = fun();
f(); //10
```



### 垃圾收集

通常，函数调用完成后，会将词法环境和其中的所有变量从内存中删除。因为现在没有任何对它们的引用了。

...

V8特性?...





## 立即调用的函数表达式（IIFE）

有时需要在定义函数之后，立即调用该函数，可以这样做：

用圆括号()将function语句包裹，然后再加上()调用（让JS引擎将函数理解为表达式而不是语句就可以调用）

```js
      //有时候定义函数后需要立即调用函数
      //这时在后面加()是不行的
      /*      function() {
        console.log("f1");
      }(); */ //报错 这里function是语句

      //在这里funciton是表达式，可以加（）直接调用
      let callF2 = (function f2() {
        console.log("f2");
      })(); //这样就可以，且直接调用了函数

      //函数f1 在function外面加上一层（）包裹，让引擎将其视为表达式
      (function() {
        console.log("f1");
      })(); //f1 调用成功
```

通常情况下，只对**匿名函数**使用立即执行，目的是：

- 不必为函数命名，**避免污染全局变量**
- IIFE内部形成了一个**单独的作用域**，可以封装一些外部无法读取的私有变量。

```js
// 写法一
var tmp = newData;
processData(tmp);
storeData(tmp);

// 写法二
(function () {
  var tmp = newData;
  processData(tmp);
  storeData(tmp);
}());

//上面代码中，写法二比写法一更好，因为完全避免了污染全局变量。
```





## eval()

`eval()` 函数会将传入的字符串当做 JavaScript 代码进行执行。

`eval()` 是全局对象的一个函数属性。

MDN: [永远不要使用 `eval`！](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval#don.27t_use_eval.21)

> 总之，`eval`的本质是在当前作用域之中，注入代码。由于安全风险和不利于 JavaScript 引擎优化执行速度，一般不推荐使用。
>
> 前面说过`eval`不利于引擎优化执行速度。更麻烦的是，还有下面这种情况，引擎在静态代码分析的阶段，根本无法分辨执行的是`eval`。









## 高阶函数

什么叫高阶函数？

**高阶函数**是对其他函数进行操作的函数，它接收函数作为参数或将函数作为返回值输出。

函数也是一种数据类型，同样可以作为参数，典型的就是回调函数。

```javascript
//callback是一个函数,作为fn的第3个参数
function fn(a, b, callback) {
  return a + b;
  callback && callback();
}
fn(1, 2, function() {
  //...
});
```







## 递归和堆栈

函数会调用 **自身**。这就是所谓的 **递归**。

```js
function pow(x, n) {
  return (n == 1) ? x : (x * pow(x, n - 1));
}
```

最大的嵌套调用次数（包括首次）被称为 **递归深度**。在我们的例子中，它正好等于 `n`。



**执行上下文和堆栈**

当一个函数进行嵌套调用时，将发生以下的事儿：

- 当前函数被暂停；
- 与它关联的执行上下文被一个叫做 **执行上下文堆栈** 的特殊数据结构保存；
- 执行嵌套调用；
- 嵌套调用结束后，从堆栈中恢复之前的执行上下文，并从停止的位置恢复外部函数。

递归深度等于堆栈中上下文的最大数量。

**任何递归都可以用循环来重写。通常循环变体更有效。**











## 函数里的this

- 全局环境下`this`就是window对象的引用
- 使用严格模式时在全局函数内`this`为`undefined`

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210507203931790.png" alt="image-20210507203931790" style="zoom:50%;" />



**对象方法调用**

函数为对象的方法时`this` 指向**该对象**



**构造函数**

函数当被 `new` 时即为构造函数，一般构造函数中包含属性与方法。函数中的上下文指向到**实例对象**。

- 构造函数主要用来生成对象，里面的this默认就是指当前对象

**对象字面量**

- 下例中的hd函数不属于对象方法所以指向`window`
- show属于对象方法执向 `obj`对象

```js
let obj = {
  site: "后盾人",
  show() {
    console.log(this.site); //后盾人
    console.log(`this in show method: ${this}`); //this in show method: [object Object]
    function hd() {
      console.log(typeof this.site); //undefined
      console.log(`this in hd function: ${this}`); //this in hd function: [object Window]
    }
    hd();
  }
};
obj.show();
```

在方法中使用函数时有些函数可以改变this如`forEach`，当然也可以使用后面介绍的`apply/call/bind`

也可以在父作用域中定义引用`this`的变量:

```js
let Lesson = {
    site: "后盾人",
    lists: ["js", "css", "mysql"],
    show() {
      const self = this;
      return this.lists.map(function(title) {
        return `${self.site}-${title}`;
      });
    }
  };
  console.log(Lesson.show());
```

### 箭头函数

箭头函数没有`this`, 也可以理解为箭头函数中的`this` 会继承定义函数时的上下文，可以理解为和外层函数指向同一个this。

- 如果想使用函数定义时的上下文中的this，那就使用箭头函数





## apply/call/bind

**apply**

```js
func.call(context, arg1, arg2, ...)
```

它运行 `func`，提供的第一个参数作为 `this`，后面的作为参数（arguments）。



**call**

```js
func.apply(context, args)
```

它运行 `func` 设置 `this=context`，并使用**类数组**对象 `args` 作为参数列表（arguments）。

`call` 和 `apply` 之间唯一的语法区别是，`call` 期望一个参数列表，而 `apply` 期望一个包含这些参数的类数组对象。

```js
func.call(context, ...args); // 使用 spread 语法将数组作为列表传递
func.apply(context, args);   // 与使用 call 相同
```



### call()

**调用函数**，并修改this指向。

主要作用是实现继承。如上面改变子类内父类的this指向

```javascript
fun.call(thisArg, arg1, arg2, ...)
```

```javascript
function Father(uname, age, gender) {
  this.uname = uname;
  this.age = age;
  this.gender = gender;
}

function Son(uname, age, gender) {
  //子构造函数继承父构造函数的属性
  Father.call(this, uname, age, gender); //这里的this就修改为 Son的this，也就是Son的实例对象
}
```



### apply()

**调用函数**， 并修改this指向。

第二个参数必须是**数组**（伪数组）。

返回值就是函数的返回值

```javascript
fun.apply(thisArg, [argsArray])
```

```javascript
//应用
//利用Math对象求最大值、最小值 Math.max() Math.min()
let arr = [2, 3, 45, 67, 43, 2, 23];
// Math.max() 函数，求最大值的方法
let maxNum = Math.max.apply(Math, arr);
let minNum = Math.min.apply(Math, arr);
console.log(maxNum, minNum); //67 2
```



### bind()

**不会调用原来的函数**

可以改变原函数内部的this指向

**返回**的是原函数改变this之后产生的**新函数**

```javascript
fun.bind(thisArg, arg1, arg2, ...) 
```

```javascript
let obj = {
  name: 'Fei'
};
function fn(a, b) {
  console.log(this);
}
let f = fn.bind(obj, 2, 3);
f(); // this指向了obj
```

如果有的函数不需要立即调用，但是又需要改变这个函数内部的this指向，用bind

案例：

获取验证码按钮，点击之后，状态变为不可操作，60s后再开启按钮

```javascript

```



## 装饰器

**装饰器**是一个围绕改变函数行为的包装器。

装饰器可以被看作是可以添加到函数的 “features” 或 “aspects”。我们可以添加一个或添加多个。而这一切都无需更改其代码！





## 箭头函数

一个参数

可省略圆括号

函数体内只有一句代码，可省略花括号

`let greeting = name => return name`



两个参数

```javascript
let greeting = (name, age) => {

		console.log(name, age);	

		return name; }

​	

无参数

​```javascript
let greeting = () => {...}
```



## 严格模式

### 变量

- 禁止不声明直接赋值
- 严禁删除已经声明的变量，如delete xxx

### this指向问题

- 以前全局作用域的this执行window，严格模式下this指向 undefined
- 严格模式下，构造函数不用new调用，this会报错

### 函数

- 同一个函数参数不能重名
- 函数必须声明在顶层，不能声明在块级作用域 ？？





# 对象

## 基础

对象是JavaScript语言的核心概念，也是最重要的数据类型。

对象就是一组“键值对”(key-value)的集合，是一种无序的复合数据集合。

### 显式创建对象 

**new Object()**

**字面量`{}`**



**键名**

对象的每一个键名又称为“属性”（property），它的“键值”可以是任何数据类型。如果一个**属性的值为函数**，通常把这个属性称为“方法”，它可以像函数那样调用。



属性命名没有限制。对象的属性名称并不像变量那样受保留字等限制。

属性名可以是任何**字符串**或者 symbol（一种特殊的标志符类型）。

其他类型会被自动地转换为**字符串**。

```js
let obj = {
  foo: "hello",
  bar: "world"
};

//{}定义了一个对象，被赋值给了变量obj，所以变量obj就指向了一个对象
```





### 表达式还是语句？

```js
{ foo: 123 }
//为了避免这种歧义，JavaScript 引擎的做法是，如果遇到这种情况，无法确定是对象还是代码块，一律解释为代码块。

//如果要解释为对象，最好在大括号前加上圆括号。因为圆括号的里面，只能是表达式，所以确保大括号只能解释为对象。
({ foo: 123 }) // 正确
({ console.log(123) }) // 报错
```





## 属性的操作

### 点语法 和 中括号(计算属性)

一般使用**点语法**可以方便的存取属性`obj.property` ，点符号要求`key`是有效的变量标识符。

当属性名不能使用点符号来操作，或者属性名需要引用一个变量，需要使用**中括号**。`obj["property"]`  或者 `obj[varWithKey]`

**计算属性**，中括号里可以是任何表达式，可以在对象字面量中完成**动态属性赋值**。中括号包围的对象属性键告诉运行时将其作为**JS表达式**而不是字符串来求值。

```js

let key = "is a red bird";
let obj = {
  ["name"]: "bird",
  [key]: true, //可以直接在字面量里面使用方括号来完成动态属性赋值
};
//obj = {
//name: "bird",
//"is a red bird": true
//}
```



### **delete 操作符**

移除属性

```js
delete obj.age;
```



### 属性值简写

开发中，通常用已经存在的变量当作属性名，比如可以用可以用 `name` 来代替 `name:name`：

```js
function makeUser(name, age) {
  return {
    name, // 与 name: name 相同
    age,  // 与 age: age 相同
    // ...
  };
}

//简写与正常的混用
let user = {
  name,  // 与 name:name 相同
  age: 30
};
```







### 属性检测 in操作符

**in操作符**

```js
"key" in object
```



```js
let user = { name: "John", age: 30 };

alert( "age" in user ); // true，user.age 存在
alert( "blabla" in user ); // false，user.blabla 不存在。
```



读取不存在的属性只会得到 `undefined`

有一种情况，存在的属性赋值为undefined，(但这种情况较少) 就无法区分是否存在，使用 in 可以检测出来。

```js
let obj = {
  test: undefined
};

alert( obj.test ); // 显示 undefined，所以属性不存在？

alert( "test" in obj ); // true，属性存在！
```







## 对象的引用和复制

与原始类型相比，对象的根本区别之一是**对象是“通过引用”被赋值和拷贝的**，与原始类型值相反：字符串，数字，布尔值等 —— 始终是以“整体值”的形式被复制的。

**赋值了对象的变量存储的不是对象本身，而是该对象“在内存中的地址”，换句话说就是对该对象的“引用”。**

拷贝此类变量或将其作为函数参数传递时，所拷贝的是引用，而不是对象本身。



### 通过引用来比较

仅当两个对象为同一对象时，两者才相等。

```js
let a = {};
let b = a; // 复制引用

alert( a == b ); // true，都引用同一对象
alert( a === b ); // true


-----------
//两个完全一样的独立的对象是不相等的
let a = {}; 
let b = {}; // 两个独立的对象

alert( a == b ); // false
```



### 合并对象 与 克隆 Object.assign（浅拷贝）

把一个对象赋值给另一个对象，只是把地址引用过来，并没有克隆一份。

如何复制独立的拷贝呢？（比较少用）

- 可以利用循环把对象的所有属性都放到一个新的对象中
- Object.assign

```js
//利用循环
let user = {
  name: "John",
  age: 30
};

let clone = {}; // 新的空对象

// 将 user 中所有的属性拷贝到其中
for (let key in user) {
  clone[key] = user[key];
}

// 现在 clone 是带有相同内容的完全独立的对象
clone.name = "Pete"; // 改变了其中的数据

alert( user.name ); // 原来的对象中的 name 属性依然是 John

```



**Object.assign**

```js
Object.assign(dest, src1, src2, src3, ...)
```

- 第一个参数 `dest` 是指目标对象。
- 更后面的参数 `src1, ..., srcN`（可按需传递多个参数）是**源对象**。
- 该方法将所有源对象的属性拷贝到目标对象 `dest` 中。换句话说，从第二个开始的所有参数的属性都被拷贝到第一个参数的对象中。
- 调用结果返回 `dest`。

所以，它还可以用来**合并多个对象**：

- 就是把源对象所有的本地属性一起复制到目标对象上



如果拷贝的属性的属性名已经存在，会覆盖掉前面的。

```js
//直接克隆
let user = {
  name: "John",
  age: 30
};

let clone = Object.assign({}, user); //把user克隆到一个空对象里，并返回这个新的对象

let user1 = {
  name: "Fei",
  job: "engineer", //我更喜欢 Designer这个称呼呢
};

let merge = Object.assign(user1, user);
```



### 深层克隆

对象里的属性的属性值，也可以是引用类型数据，所以只简单对对象的拷贝是不够的，引用类型的属性值依然是拷贝的引用地址。

```js
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = Object.assign({}, user);

alert( user.sizes === clone.sizes ); // true，同一个对象

// user 和 clone 分享同一个 sizes
user.sizes.width++;       // 通过其中一个改变属性值
alert(clone.sizes.width); // 51，能从另外一个看到变更的结果
```

为了解决此问题，我们应该使用会检查每个 `user[key]` 的值的克隆循环，如果值是一个对象，那么也要复制它的结构。这就叫“深拷贝”。

我们可以用递归来实现。或者不自己造轮子，使用现成的实现，例如 JavaScript 库 [lodash](https://lodash.com/) 中的 [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep)。



### 垃圾回收

- 垃圾回收是自动完成的
- 当对象是可达性状态时，它一定是存在于内存中的
- 被引用和可访问不同：一组相互连接的对象可能整体都不可达



## 对象里的方法

我们通常创建对象来表示真实世界中的实体，用属性中的函数来表示对象中的行为。

作为对象属性的函数被称为 **方法**。

### 方法简写

```js
//在给对象定义方法时，通常写一个方法名，冒号，然后再引用一个匿名函数表达式
let user = {
  sayHi: function() {
    alert("Hello");
  }
};

// 新的简写方法
let user = {
  sayHi() { // 与 "sayHi: function()" 一样
    alert("Hello");
  }
};

//简写方法名与可计算属性键相互兼容
const methodKey = 'sayName';
let person = {
  [methodKey](name) { //一样可以
    ///
  }
};
```



### 对象方法中的this

对象方法需要访问对象中存储的信息才能完成其工作。

this指向调用方法的对象

`this` 的值是在代码运行时计算出来的，它取决于代码上下文。



### 实现对象方法的链式调用

在每次调用方法后让其返回到对象本身：

```js
 //3.链式调用
      //有一个可以上下移动的 ladder 对象：
      let ladder = {
        step: 0,
        up() {
          this.step++;
          //return ladder;
          return this; //调用函数后返回对象本身
        },
        down() {
          this.step--;
          //return ladder;
          return this;
        },
        showStep: function () {
          // 显示当前的 step
          console.log(this.step);
          //return ladder;
          return this;
        },
      };
      //ladder.up();
      //ladder.up();
      //ladder.down();
      //ladder.showStep(); // 1

      //怎么修改代码可以做到链式调用？
      //解决方案就是：在每次调用后再返回这个对象本身
      ladder.up().up().down().showStep(); // 1
```





## 对象的属性类型

>  JavaScript提供了一个内部数据结构，用来描述对象的属性，控制它的行为，比如该属性是否可写、可遍历等。这个内部数据结构称为“属性描述对象”。

ECMA-262 使用一些内部特性来描述属性的特征，用两个中括号包裹，如`[[Writable]]`

对象的属性分为两种：**数据属性 **和 **访问器属性**

### 数据属性(元属性)

数据属性包含一个保存数据值的位置。数据属性有4个特性描述他们的行为：

- `configurable`— 决定是否可以修改属性描述对象。如果为 `true`，则特性可以被删除，这些属性也可以被修改，否则不可以。
- **`enumerable`** — 如果为 `true`，则会被在循环中列出，否则不会被列出。
- **`writable`** — 如果为 `true`，则值可以被修改，否则它是只可读的。
- **`value`** — 包含属性实际的值。

我们通常创建对象的一个属性时，就默认创建了上面4个特性， 除value外三个特性的默认值都为true





#### **读取属性的特性**

**Object.getOwnPropertyDescriptor**

读取某个属性的描述对象

```js
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);
```

```js
let user = {
  name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/* 属性描述符：
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```

`Object.getOwnPropertyDescriptors` 返回包含 symbol 类型的属性在内的 **所有** 属性描述符。





### Object.defineProperty

通过描述对象，定义某个属性或修改属性的特性。

```js
Object.defineProperty(obj, propertyName, descriptor)
```

```js
let person = {};
Object.defineProperty(person, "name", {
  writable: false,
  configurable: false, 
  value: "Fei",
});
```

如果一个属性的value在修改特性时才创建，又没有特殊指定特性的值，那么它会使用给定的值和标志创建属性，没有提供值的标志都会默认为false.

它与 `Object.defineProperties` 一起可以用作克隆对象的“标志感知”方式：

```js
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```







### Object.prototype.propertyIsEnumerable()

用于判断某个属性是否可遍历，返回一个布尔值。

该方法只能用于判断对象自身的属性，对于继承的属性一律返回false。

```js
      //查看属性是否可遍历
      //Object.prototype.propertyIsEnumerable()
      console.log(obj.propertyIsEnumerable("name")); //true
      console.log(obj.propertyIsEnumerable("age")); //false
```



### 访问器属性（存取器）

accessor properties

访问器属性不包含数据值，而是包含一个getter函数（读取）、一个setter函数（设置）。

本质上是用于获取和设置值的函数。

在**读取**访问器属性时，会调用getter函数，getter函数会返回一个有效的值；

在**写入**访问器属性时，会调用setter函数并传入新值，setter函数必须决定对数据作出什么修改。

访问器属性的4个特性：

- **`get`** —— 一个没有参数的函数，在读取属性时工作，
- **`set`** —— 带有一个参数的函数，当属性被设置时调用，
- **`enumerable`** —— 与数据属性的相同，
- **`configurable`** —— 与数据属性的相同。



```js
      //也叫存取器属性
      //可以在定义对象的时候直接设置存取器属性
      let computer = {
        name: "Macbook pro",
        service: 0,
        _year: 2015,

        //设置存取器属性year
        //读取时返回getter函数
        get year() {
          return this._year;
        },
        //写入时返回setter函数
        //只能有一个参数
        set year(value) {
          this._year = value;
          this.service = value - 2015;
        },
      };

      //读取
      console.log(computer._year); //2015
      console.log(computer.year); //2015
      console.log(computer.service); //0

      //写入
      computer.year = 2021;
      console.log(computer._year); //2021
      console.log(computer.year); //2021
      console.log(computer.service); //6

      console.log(Object.getOwnPropertyDescriptor(computer, "year"));
      //{enumerable: true, configurable: true, get: ƒ, set: ƒ}
```



可以通过`Object.defineProperty()` 或者 `Object.defineProperties()`来给已存在的对象添加setter/getter属性：

```js
 //访问器属性
      let obj = {
        name: "John",
        surname: "Smith",
      };

      //给obj添加一个访问器属性 fullName
      Object.defineProperty(obj, "fullName", {
        get() {
          //get函数没有参数，在读取时调用
          return this.name + " " + this.surname;
        },
        set(value) {
          //解构
          [this.name, this.surname] = value.split(" ");
          //下面有写入的代码 obj.fullName = 'Neo Wong'
          //相当于
          //this.name = value.split(" ")[0];
          //this.surname = value.split(" ")[1];
        },
      });
      //读取
      console.log(obj.fullName); //John Smith

      //写入
      obj.fullName = "Neo Wong";
      console.log(obj.fullName); //Neo Wong
```

一个属性要么是访问器（具有 `get/set` 方法），要么是数据属性（具有 `value`），但不能两者都是。

访问器的一大**用途**是，它们允许随时通过使用 getter 和 setter 替换“正常的”数据属性，来控制和调整这些属性的行为。

```js
//案例
//想象一下，我们开始使用数据属性 name 和 age 来实现 user 对象：
function User(name, age) {
  this.name = name;
  this.age = age;
}

let john = new User("John", 25);

alert( john.age ); // 25

//……但迟早，情况可能会发生变化。我们可能会决定存储 birthday，而不是 age，因为它更精确，更方便：
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;
}

let john = new User("John", new Date(1992, 6, 1));

//现在应该如何处理仍使用 age 属性的旧代码呢？

//我们可以尝试找到所有这些地方并修改它们，但这会花费很多时间，而且如果其他很多人都在使用该代码，那么可能很难完成所有修改。而且，user 中有 age 是一件好事，对吧？

//那我们就把它保留下来吧。

//为 age 添加一个 getter 来解决这个问题：
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;

  // 年龄是根据当前日期和生日计算得出的
  Object.defineProperty(this, "age", {
    get() {
      let todayYear = new Date().getFullYear();
      return todayYear - this.birthday.getFullYear();
    }
  });
}

let john = new User("John", new Date(1992, 6, 1));

alert( john.birthday ); // birthday 是可访问的
alert( john.age );      // ……age 也是可访问的
```







### Object.defineProperties

通过描述对象，定义多个属性

```js
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false },
  // ...
});
```

```js
      //Object.defineProperties
      let computer = {};
      Object.defineProperties(computer, {
        //数据属性
        owner: {
          value: "Fei",
        },
        password: {
          value: 567223,
        },
        year_: {
          value: 2015,
          writable: true,
        },
        service: {
          value: 0,
          writable: true,
        },

        //访问器属性
        year: {
          get() {
            return this.year_;
          },
          set(value) {
            this.year_ = value;
            this.service = value - 2015;
          },
        },
      });

      console.log(computer.year); //2015
      computer.year = 2021;
      console.log(computer.year); //2021
      console.log(computer.service); //6
```





### 控制对象状态

有时需要冻结对象的读写状态，防止对象被改变。JavaScript 提供了三种冻结方法，最弱的一种是`Object.preventExtensions`，其次是`Object.seal`，最强的是`Object.freeze`。



`Object.preventExtensions`方法可以使得一个对象无法再添加新的属性。

`Object.isExtensible`方法用于检查一个对象是否使用了`Object.preventExtensions`方法。也就是说，检查是否可以为一个对象添加属性。



`Object.seal`方法使得一个对象既无法添加新属性，也无法删除旧属性。

`Object.isSealed`方法用于检查一个对象是否使用了`Object.seal`方法。



`Object.freeze`方法可以使得一个对象无法添加新属性、无法删除旧属性、也无法改变属性的值，使得这个对象实际上变成了常量。

`Object.isFrozen`方法用于检查一个对象是否使用了`Object.freeze`方法。



上面的三个方法锁定对象的可写性有一个漏洞：可以通过改变原型对象，来为对象增加属性。

另外一个局限是，如果属性值是对象，上面这些方法只能冻结属性指向的对象，而不能冻结对象本身的内容。





## 对象的迭代遍历

### Object.keys(), values(), entries()

对于普通对象，下列这些方法是可用的：

- [Object.keys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) —— 返回一个包含该对象所有的键的**数组**。
- [Object.values(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/values) —— 返回一个包含该对象所有的值的**数组**。
- [Object.entries(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) —— 返回一个包含该对象所有 **[key, value] 键值对的数组**。

这几个方法会忽略Symbol类型的键，如果要遍历对象里的Symbol数据，则使用：

- [Object.getOwnPropertySymbols](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols)，它会返回一个只包含 Symbol 类型的键的数组。
- 另外，还有一种方法 [Reflect.ownKeys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys)，它会返回 **所有** 键



|          | Map          | Object                                  |
| :------- | :----------- | :-------------------------------------- |
| 调用语法 | `map.keys()` | `Object.keys(obj)`，而不是 `obj.keys()` |
| 返回值   | 可迭代项对象 | “真正的”数组                            |





### Object.getOwnPropertyNames()

和`Object.keys()`一样，是用来遍历对象的属性的，但有一个区别：

- `Object.keys()`只返回可枚举的属性，`Object.getOwnPropertyNames()` 还可以返回不可枚举的属性名。

- ```js
        //Object.keys() 和 Object.getOwnPropertyNames()
        //前者只可遍历可枚举属性名，后者还可遍历不可枚举属性名
    
        let arr = ["a", "b"]; //定义一个数组，他默认键名就是index，length是不可枚举属性
    
        //用Object.keys()
        console.log(Object.keys(arr)); //["0", "1"]
    
        //用Object.getOwnPropertyNames()
        console.log(Object.getOwnPropertyNames(arr)); //["0", "1", "length"]
    
        //案例2
    			//由于age, gender, job这三个属性默认设置了不可枚举，所以用两种方法遍历出的keys的结果是不一样的
        let obj = { name: "Fei" };
        Object.defineProperty(obj, "age", {
          value: 30,
        });
        //定义多个属性
        Object.defineProperties(obj, {
          gender: {
            value: "Female",
          },
          job: {
            value: "Designer",
          },
        });
    
        //遍历obj的keys
        console.log(Object.keys(obj)); //["name"]
        console.log(Object.getOwnPropertyNames(obj)); //["name", "age", "gender", "job"]
    
        //查看属性个数
        console.log(Object.keys(obj).length); //1
        console.log(Object.getOwnPropertyNames(obj).length); //4
  ```

  







### for...in

- 它遍历的是对象所有可遍历（enumerable）的属性，会跳过不可遍历的属性。
- 它不仅遍历对象自身的属性，还遍历继承的属性。

```js
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // keys
  alert( key );  // name, age, isAdmin
  // 属性键的值
  alert( user[key] ); // John, 30, true
}


//如果只遍历自身的属性，应该结合使用hasOwnProperty
var person = { name: '老张' };

for (var key in person) {
  if (person.hasOwnProperty(key)) {
    console.log(key);
  }
}
// name
```

**遍历到的属性的顺序：**

“有特别的顺序”：**整数属性**会被进行排序，其他属性则按照**创建的顺序**显示。

（这里的“整数属性”指的是一个可以在不做任何更改的情况下与一个整数进行相互转换的字符串。

所以，“49” 是一个整数属性名，因为我们把它转换成整数，再转换回来，它还是一样的。但是 “+49” 和 “1.2” 就不行了：）



案例：

为了解决电话号码的问题，我们可以使用非整数属性名来 **欺骗** 程序。只需要给每个键名加一个加号 `"+"` 前缀就行了。

```javascript
let codes = {
  "+49": "Germany",
  "+41": "Switzerland",
  "+44": "Great Britain",
  // ..,
  "+1": "USA"
};

for (let code in codes) {
  alert( +code ); // 49, 41, 44, 1
}
```







## 对象的原生方法归总

Object对象的原生方法分为两类：Object本身的方法和Object的实例方法。

Object 对象**自身的方法(静态方法)**：就是**直接定义**在Object对象的方法：

```js
      Object.print = function (o) {
        console.log(o);
      };
      Object.print("a"); //a
```



Object的**实例方法**：就是定义在**Object.prototype**上的方法，可以被Object的实例直接使用。

```js
      Object.prototype.show = function () {
        console.log(this);
      };
      let obj = {};//Object的实例obj
      obj.show(); //{} //obj继承了Object.prototype上的方法
```



**Object()**

可以当作工具方法使用，将任意值转为对象。





### Object的静态方法

所谓静态方法，是指部署在object对象自身的方法。

- 遍历对象的属性
  - `Object.keys()`：遍历可枚举的属性名
  - `Object.getOwnPropertyNames()`：遍历所有属性名包括不可枚举的
- 对象属性特性的相关方法
  - `Object.getOwnPropertyDescriptor()`：获取某个属性的描述对象。
  - `Object.defineProperty()`：通过描述对象，定义某个属性。
  - `Object.defineProperties()`：通过描述对象，定义多个属性。
- 控制对象状态的方法：
  - `Object.preventExtensions()`：防止对象扩展。
  - `Object.isExtensible()`：判断对象是否可扩展。
  - `Object.seal()`：禁止对象配置。
  - `Object.isSealed()`：判断一个对象是否可配置。
  - `Object.freeze()`：冻结一个对象。
  - `Object.isFrozen()`：判断一个对象是否被冻结。
- 原型链相关方法：
  - `Object.create()`：该方法可以指定原型对象和属性，返回一个新的对象。
  - `Object.getPrototypeOf()`：获取对象的`Prototype`对象。



### Object的实例方法

也就是定义在Object.prototype上的方法，主要有以下6个：

- `Object.prototype.valueOf()`：返回当前对象对应的值。
- `Object.prototype.toString()`：返回当前对象对应的字符串形式。
- `Object.prototype.toLocaleString()`：返回当前对象对应的本地字符串形式。
- `Object.prototype.hasOwnProperty()`：判断某个属性是否为当前对象自身的属性，还是继承自原型对象的属性。
- `Object.prototype.isPrototypeOf()`：判断当前对象是否为另一个对象的原型。
- `Object.prototype.propertyIsEnumerable()`：判断某个属性是否可枚举。

```js
      //Object的实例方法
      //Object.prototype.valueOf()
      //默认情况下返回对象本身
      //主要用途：自动类型转换时会默认调用该方法
      console.log(obj.valueOf()); //{name: "Fei", age: 30, gender: "Female", job: "Designer"}
      console.log(obj); //{name: "Fei", age: 30, gender: "Female", job: "Designer"}
      console.log(obj.valueOf() === obj); //true

      //Object.prototype.toString()
      //返回类型字符串
      console.log(obj.toString()); //"[object Object]"
      //可以自定义toString()方法，覆盖原型上的同String()来得到需要的结果
      obj.toString = function () {
        return "Hello";
      };
      console.log(obj + " " + "Fei"); //"Hello Fei"
      //在上面表达式中，obj会自动转换成字符串，调用obj.toString()

      // 数组、字符串、函数、Date 对象都分别部署了自定义的toString方法，覆盖了Object.prototype.toString方法。
      let num = 1;
      console.log(num.toString()); //1
      //toString()还可用来判断数据类型
      //由于实例对象可能自定义了toString()方法，要通过call()方法直接调用实例上的toString()来得到数据类型
      console.log(Object.prototype.toString.call(num)); //"[object Number]"
      console.log(Object.prototype.toString.call([1, 2])); //"[object Array]"
```





## with 语句

不经常使用，有弊端。

MDN的**警告**：

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210601163533875.png" alt="image-20210601163533875" style="zoom:50%;" />

它的作用是操作同一个对象的多个属性时，提供一些书写的方便。

```js
with (对象) {
  语句;
}
```

```js
// 例一
var obj = {
  p1: 1,
  p2: 2,
};
//通常操作语句方法：
obj.p1 = 4;
obj.p2 = 5;

//用with语句操作属性:
with(obj) {
  p1 = 4;
  p2 = 5;
}
//with 语句操作的属性，必须是对象里已经存在的属性，否则会创造一个全局变量。
```









## 转换类型

**转换成数组**

对象缺少数组的许多方法，如map(), filter()等，可以转换后再使用

可以使用`Object.entries	`获取对象的键值对数组，在处理完数据后，再使用`Object.fromEntries`将数组数据转换为对象

```js
let prices = {
        banana: 1,
        orange: 2,
        meat: 4,
      };

let newPrice = Object.fromEntries(
    Object.entries(prices).map(([key, value]) => [key, value * 2])
      );
console.log(newPrice); //{banana: 2, orange: 4, meat: 8}

```













# 面向对象编程

## 介绍

面向对象是把事物分解成为一个一个对象，然后对象之间分工与合作，完成对真实世界的模拟。

（以功能划分问题，而不是步骤）

要先找出所有对象，并写出这些对象的功能



**面向对象编程的优点**

灵活、代码可复用、容易维护和开发，更适合多人合作的大型软件项目。



面向过程编程：

- 性能比面向对象高，适合跟硬件联系密切的东西，如单片机
- 没有面向对象易维护、易扩展



面向对象编程OOP的特性

- 封装性
- 继承性
- 多态性

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210506092530422.png" alt="image-20210506092530422" style="zoom:50%;" />







## 构造函数与实例对象

面向对象编程的第一步，就是要生成对象。通常需要一个模板，表示某一类实物的共同特征。



JavaScript语言的对象体系，不是基于类，而是基于**构造函数**和**原型链**。

JavaScript语言使用构造函数作为对象的模板。所谓构造函数，就是专门用来生成实例对象的**函数**。



构造函数在技术上是常规函数。不过有两个约定：

1. 它们的命名以大写字母开头。
2. 函数内部使用了`this`关键字，代表了所要生成的对象实例。
3. 生成对象的时候，必须使用 `new` 命令。





>  在典型的面向对象的语言中，都存在类的概念，类就是对象的模板，但在JavaScript中 ES6 之前，并没有引入类的概念。
>
> 在ES6之前，对象不是通过class创建的，而是用一种称为**构造函数**的特殊函数来定义对象和它们的特征。



> #### 静态成员和实例成员
>
> 构造函数中的属性和方法，称为成员。
>
> **静态成员**：在构造函数本身上添加的成员称为静态成员，只能由构造函数本身来访问
>
> **实例成员**：在构造函数内部创建的对象（通过this添加的）成员称为实例成员，只能由实例化的对象来访问



复习-创建对象的方法：

- 对象字面量{}
- new Object()
- 工厂模式（没有标志）
- 构造函数
- class





### new 创建实例对象

利用构造函数将公共属性和方法抽离出来封装成一个构造函数模板。

在创建对象时，使用 **new** 创建对象实例 ，`new`后面的函数依次执行下面的步骤：

- 在内存中创建一个新的空对象，作为将要返回的对象实例
- 将这个空对象的原型，指向构造函数的`prototype`属性
- 将这个空对象赋值给函数内部的`this`关键字
- 执行构造函数里面的代码，给这个新对象添加属性和方法
- 返回这个对象（构造函数里不需要return）



严格模式下，如果不用 `new`，直接调用构造函数将会报错。





#### new.target

函数内部可以使用`new.target`属性。

如果当前函数是`new`命令调用，则`new.target`指向当前函数。

```js
function F() {
  console.log(new.target === F);
}
F(); //false
new F(); //true
```







### Object.create() 创建实例对象

构造函数作为模板，可以生成实例对象。但是有时拿不到构造函数，只能拿到一个**现有的对象**。我们希望以该对象为模板，生成新的实例对象，这是就要使用`Object.create()`。

```js
      //Object.create()
      //有时拿不到构造函数，想要以现有的一个对象为模板，创建一个新的对象
      let person1 = {
        name: "Fei",
        age: 30,
        gender: "female",
        skill() {
          console.log(this.name + " skill");
        },
      };

      let person2 = Object.create(person1);
      console.log(person1); //{name: "Fei", age: 30, gender: "female"}
      console.log(person2); //{}
      //如果不加参数，新的对象本身是一个空对象
      //它的属性和方法都是从person1继承来的
      console.log(person2.name); //Fei
      person2.skill(); //Fei skill

      //Object.create()的第二个参数是可选的，是一个对象，写法与Object.defineProperties()一样
      let person3 = Object.create(person1, {
        name: {
          value: "Harry",
        },
      });
      console.log(person3);
      console.log(person3.name); //Harry
      person3.skill(); //Harry skill
```









## this 关键字

`this`非常重要，应用场景也很多，但不管什么场合，`this`都有一个共同点：它总是**返回一个对象**。

也就是说，`this`就是属性和方法"当前"所在的对象。

`this`的指向是动态的。



### this 的设计目的

```js
let obj = { foo: 5 };
//这行代码将一个对象 {foo: 5} 赋值给变量 obj。
//在执行时，JS引擎会现在内存里生存一个对象{foo: 5}，然后把这个对象的内存地址赋值给变量obj。 变量 obj 只是引用了这个地址。
//当要读取变量obj时，JS引擎会先从obj拿到它所指向的内存地址，然后再从该地址读出原始的对象，返回它的foo属性。
```

由于函数可以在不同的运行环境执行，所以需要有一种机制，能够在函数体内部获得当前的运行环境，`this`的设计目的就是在函数体内部，指代**函数当前的运行环境**。

```js
var f = function () {
  console.log(this.x);
}

var x = 1;
var obj = {
  f: f,
  x: 2,
};

// 单独执行
f() // 1

// obj 环境执行
obj.f() // 2
```





### this 的使用场合

1. 全局环境 指向window，严格模式undefined
2. 对象的方法中 指向方法运行时所在的对象（调用者）
3. 构造函数中 指向实例对象
4. 超时函数中 指向window
5. 立即执行函数 指向window
6. 事件监听函数中 指向事件绑定的对象
7. 显式绑定（硬绑定）



**注意**

- 避免多层this
- 避免数组处理方法中的this
- 避免回调函数中的this





### 绑定this的方法

#### call()

指定函数内部`this`的指向，并调用该函数。

**调用对象的原生方法**

```js
Object.prototype.hasOwnProperty.call(obj, 'toString');
```



#### apply()

接收一个数组作为函数执行时的参数

```js
      //this 的硬绑定
      function f1() {
        console.log(this.name);
      }
      let obj = {
        name: "Fei",
      };
      //通过call，修改this，并调用函数
      f1.call(obj); //Fei

      //传参数
      function sum(a, b) {
        console.log(a + b);
      }
      sum(1, 2); //3
      sum.call(this, 1, 2); //3
      sum.apply(this, [1, 2]); //3
      sum.call(null, 1, 2); //3

      //apply与call的区别在于参数
      //使用apply调用Math.max函数 找出数组中最大的值
      let max = Math.max.apply(null, [1, 23, 43, 54]);
      console.log(max); //54
      //将数组的空元素变成undefined
      //通过Apply方法，利用Array构造函数
      let arr = [1, , , 3, 4];
      let newArr = Array.apply(null, arr);
      console.log(newArr); // [1, undefined, undefined, 3, 4]
```



**转换类似数组的对象**

```js
//slice方法可以用来复制一个数组
let arr 
```



#### bind()















## 原型和继承

### 原型对象概述

#### 构造函数的问题

构造函数存在**浪费内存**的问题：

其中的方法，每创建一个实例对象，都会开辟一块内存空间来存放同一个函数。（但其实构造函数里的方法代码都是一样的，只是实例对象不一样，用很多空间来存放同一个函数不科学）

我们希望所有的对象使用同一个函数，节省内存：

**构造函数原型对象 prototype**

通过原型prototype分配函数，所有对象共享。





#### prototype 属性的作用

JS中，每一个构造函数都有一个prototype属性，prototype就是一个对象，这个对象的所有属性和方法，都会被构造函数所拥有。

只要把那些不变的需要共享的方法，直接定义在 prototype对象上，所有对象的实例就可以共享这些方法了。

```javascript
function Hero(hname, age, type) {
  this.hname = hname;
  this.age = age;
  this.type = type;
  // 这个方法可以定义在prototype对象上进行共享
  /*this.attack = function(attack){
    console.log(attack);
  }*/
}

// 将方法直接定义在了prototype对象上，所有实例对象都可以直接调用
Hero.prototype.attack = function(attack){
  console.log(attack)
}
```

原型是一个对象，也叫原型对象。

原型的作用是共享方法，节省内存资源。



**对象原型  _proto__**

对象都会有一个属性 _proto__，指向构造函数的prototype原型对象。

```javascript
// 对象原型_proto_ 和 原型对象prototype 是等价的
console.log(houyi.__proto__ === Hero.prototype); //true 注意这里__proto__ 每侧是两个下划线
```

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210506105914321.png" alt="image-20210506105914321" style="zoom:50%;" />





#### constructor 属性

对象原型和原型对象里面都有一个属性constructor属性，我们称constructor为构造函数，它指回构造函数本身。

作用：

主要用于记录该对象引用于哪个构造函数，它可以让原型对象重新指向原来的构造函数。

```js
      //原型对象
      //共享属性和方法
      function Animals(name, color) {
        this.name = name;
      }
      //在原型上添加方法
      Animals.prototype.run = function () {
        console.log(this.name + " can run");
      };
      //在原型上添加属性
      Animals.prototype.color = "white";

      let cat = new Animals("cat", "white");
      let dog = new Animals("dog", "brown");
      console.log(cat.name);
      console.log(dog.name);
      console.log(cat.color); //white
      console.log(dog.color); //white
      cat.run(); //cat can run
      dog.run(); //dog can run
      //----------
      //constructor
      //prototype对象有一个constructor属性，默认指向prototype对象所在的构造函数
      console.log(Animals.prototype.constructor === Animals); //true
      //因为constructor属性是在构造函数的原型上，所以所有的实例对象都会继承constructor属性
      //实例对象自身其实没有constructor属性，而是从构造函数原型上继承过来的
      //查看实例对象cat和dog
      console.dir(Animals); //构造函数的constructor在Animals.prototype上
      console.dir(cat); //实例对象的constructor在实例对象的__proto__里
      console.log(cat.__proto__.constructor === Animals.prototype.constructor); //true
      //实例上的constructor属性的作用，可以得知某个实例对象，是从哪个构造函数产生的
      console.log(cat.constructor);
      //Animals(name, color) {
      //  this.name = name;
      //}
      console.log(cat.constructor === Animals); //true
      //可以利用constructor从一个实例对象新建另一个实例对象
      //比如从cat实例新建另一个实例对象
      let duck = new cat.constructor();
      console.log(duck); //Animals {name: undefined}
      console.log(duck instanceof Animals); //true
```



`constructor`属性表示原型对象与构造函数之间的关联关系，如果修改了原型对象，一般会同时修改`constructor`属性，防止引用的时候出错。

```js
      //如果修改了一个对象的原型对象，那么也要相应将其constructor修改
      function Super() {}
      function Subtype() {}
      console.log(Subtype.prototype.constructor); //ƒ Subtype() {}

      //修改Subtype的原型
      Subtype.prototype = new Super();
      console.log(Subtype.prototype.constructor); //ƒ Super() {}
      //将其constructor 重新指向自己
      Subtype.prototype.constructor = Subtype;
      console.log(Subtype.prototype.constructor); //ƒ Subtype() {}
```



```js
// 坏的写法
C.prototype = {
  method1: function (...) { ... },
  // ...
};

// 好的写法
C.prototype = {
  constructor: C,
  method1: function (...) { ... },
  // ...
};

// 更好的写法
C.prototype.method1 = function (...) { ... };
```





#### instanceof 运算符

表示对象是否为某个构造函数的实例，它会返回一个布尔值。

```js
      //instanceof
			//实例对象 instanceof 构造函数
			//它会检查右边构造函数的原型对象prototype，是否在左边实例对象的原型链上
      console.log(cat instanceof Animals); //true
      //等同于 isPrototypeOf()
      console.log(Animals.prototype.isPrototypeOf(cat)); //true 检查构造函数Animals的prototype原型是否是cat对象的原型
 
```

```js
var obj = Object.create(null);
typeof obj // "object"
obj instanceof Object // false
```

上面代码中，`Object.create(null)`返回一个新对象`obj`，它的原型是`null`（`Object.create()`的详细介绍见后文）。右边的构造函数`Object`的`prototype`属性，不在左边的原型链上，因此`instanceof`就认为`obj`不是`Object`的实例。这是唯一的`instanceof`运算符判断会失真的情况（一个对象的原型是`null`）。







#### 原型链

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210506172532137.png" alt="image-20210506172532137" style="zoom:50%;" />

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210507100256081.png" alt="image-20210507100256081" style="zoom:50%;" />

#### 对象成员查找规则

1. 当访问一个对象的属性、方法时，首先**查找**这个对象**自身**有没有该属性
2. 如果没有就查找它的对象原型（__proto _）指向的prototype**原型**
3. 如果还没有找到就查找**原型对象的原型**（Object.prototype）
4. 依此类推一直找到Object（**null**）为止

—proto— 对象原型的意义就在于为对象成员查找机制提供一个方向一条路线











### 继承

#### 构造函数的继承

- 在子类的构造函数中，调用父类的构造函数
- 让子类的原型指向父类的原型，子类就可以继承父类原型

```javascript
      function Shape() {
        this.x = 0;
        this.y = 0;
      }
      Shape.prototype.move = function (x, y) {
        this.x += x;
        this.y += y;
        console.info("Shape moved");
      };
      //1.子类继承父类的实例
      function Rectangle() {
        Shape.call(this); //调用父类构造函数
      }
      //2.子类继承父类的原型
      Rectangle.prototype = Object.create(Shape.prototype);
      Rectangle.prototype.constructor = Rectangle; //子类原型的constructor重新指回本身
      //创建一个子类实例
      let rect = new Rectangle();
      console.dir(rect);
      console.log(rect instanceof Rectangle); //true
      console.log(rect instanceof Shape); //true
      //---------
      //上面是子类整体继承父类
      //也可以单独继承某个方法
      ClassB.prototype.print = function () {
        ClassA.prototype.print.call(this);
        // some code
      };
```









#### Mixin 多重继承？













## Object对象的相关方法

### Object.getPrototypeOf()

`Object.getPrototypeOf`方法返回参数对象的原型。这是获取原型对象的标准方法。

```js
      //Object.getPrototypeOf()
      //获取对象的原型
      function F() {}
      let f = new F();
      //直接看，f的原型就是F.prototype
      console.log(Object.getPrototypeOf(f).constructor); //ƒ F() {}
      console.log(Object.getPrototypeOf(f) === F.prototype); //true
      //几种特殊对象的原型
      //{}, null,函数
      //{}空对象的原型是Object.prototype
      console.log(Object.getPrototypeOf({}) === Object.prototype); //true
      //Object.prototype的原型是null
      console.log(Object.getPrototypeOf(Object.prototype) === null); //true
      //函数的原型是Function.prototype
      console.log(Object.getPrototypeOf(F) === Function.prototype); //true
```







### Object.setPrototypeOf()

`Object.setPrototypeOf`方法为参数对象设置原型，返回该参数对象。它接受两个参数，第一个是现有对象，第二个是原型对象。

```js
      //Object.setPrototypeOf(a, b)
      //第一个参数是现有对象，第二个参数是原型对象
      //也就是把a的原型设置为b
      function a() {}
      function b() {}
      Object.setPrototypeOf(b, a);
      console.dir(b);
      console.log(Object.getPrototypeOf(b) === a); //true
```







### Object.create()

该方法接受一个对象作为参数，然后以它为原型，返回一个实例对象。该实例完全继承原型对象的属性。

除了对象的原型，`Object.create()`方法还可以接受第二个参数。该参数是一个属性描述对象，它所描述的对象属性，会添加到实例对象，作为该对象自身的属性。







### Object.prototype.isPrototypeOf()

实例对象的`isPrototypeOf`方法，用来判断该对象是否为参数对象的原型。







### Object.prototype.__ proto__

实例对象的`__proto__`属性（前后各两个下划线），返回该对象的原型。该属性可读写。





### Object.getOwnPropertyNames()

`Object.getOwnPropertyNames`方法返回一个数组，成员是参数对象本身的所有属性的键名，不包含继承的属性键名。











### Object.prototype.hasOwnProperty()

对象实例的`hasOwnProperty`方法返回一个布尔值，用于判断某个属性定义在对象自身，还是定义在原型链上。













## 类

1. 抽取对象公用的属性和行为组织封装成一个类（模板） （构造函数？）
2. 对类进行实例化，获取类和对象 （new 实例化构造函数？）

类抽象了对象的公共部分，它泛指某一大类（class）

对象特指某一个，通过类实例化一个具体的对象



这里我的思考：

- 类，表示抽取的对象的公共部分，就相当于之前说的对象的构造函数
- 构造函数的实例化，其实就是来实例一个对象
- 这里ES6又有class来创建类，所以class与构造函数的区别是什么？



### 创建类

```javascript
// 语法
class Star {
  // class body
}

//创建实例
let obj = new Star(); //类必须使用 new 实例化对象
```

#### 类 constructor 构造函数

constructor() 方法是类的构造函数，用于**传递参数**，返回实例对象，通过new命令生成对象实例时，会自动调用该方法。

```javascript
// 语法
// 类里面所有函数都不用写function，也不用加逗号分隔键值对
class Star { // 类名习惯性首字母大写，且类名后面不要加圆括号  
  // constructor函数可以接收参数，同时返回给实例对象
  // 只要new实例时，就会自动调用constructor函数，不写也会自动生成
  constructor(uname){ 
    this.uname = uname; // this 指向new创建的实例？
  }
   // 类中添加方法 
  skill(uskill){
      console.log(this.uname + '的技能是：' + uskill)
  }
}

//创建实例
let obj = new Star('Fei'); //类必须使用 new 实例化对象
//给类里的方法传递参数
obj.skill(sing);
```



### 类的继承 

##### extends 和 super 

extemds

子类继承父类的属性和方法

```javascript
// 父类
class Father {
	constructor() {
    //
  }
  money() {
    //
  }
}

// 子类
// 使用extnds继承父类
class Son extends Father {
  
}

// 子类就有了父类里的属性和方法
let son = new Son();
console.log(Son.money());
```

super

super 关键字可以访问和调用对象父类上的函数，可以调用父类的构造函数，也可以调用父类的普通函数。

```javascript
class Father {
  constructor(num1, num2) {
    this.num1 = num1;
    this.num2 = num2;
  }  
  //父类里的函数
  sum() { 
    console.log(this.num1 + this.num2);
  }
}

//定义子类，并使用extends继承父类
class Son extends Father {
  constructor(num1, num2) {
    //用super关键字调用父类上的构造函数
    super(num1, num2); 
  }
}

let son = new Son(1, 2);
son.num() //3
```



使用类的注意点：

- ES6中，类没有变量提升，所以必须先定义类，才能通过类实例化对象。
- 类里面的共有的属性和方法一定要加this使用 
- 类里面this的指向问题
  - constructor 里的this，指向创建的实例对象
  - 定义的函数里的this，指向的是调用这个函数的调用者



### 类的本质

类的本质其实还是一个函数，我们也可以简单的认为，类就是构造函数的另外一种写法。

ES6 之前通过 **构造函数+原型** 实现面向对象编程。

ES6 通过 **类** 实现面向对象编程。

类一样有prototype, 原型对象里也有constructor指向类本身，类也可以通过原型添加方法实现共享。

ES6 的类其实就是语法糖，新的写法只是让对象原型的写法更加清晰、更像面向对象编程的语法。







### 注意点

- 类和模块的内部，默认就是严格模式

- 类不存在变量提升

  







# 错误处理机制

## Error 实例对象

JavaScript 解析或运行时，一旦发生错误，引擎就会抛出一个错误对象。

JavaScript原生提供 **Error 构造函数**，**所有抛出的错误都是这个构造函数的实例**。



**Error 实例的属性**： 

- message 属性：错误提示信息
- name 属性：错误名称（非标准属性）
- stack 属性：错误的堆栈（非标准属性）





## 原生错误类型

JavaScript中还存在Error的6个派生对象（6种错误对象）

### SyntaxError 对象

syntax，语法。

SyntaxError 对象是解析代码时发生的 **语法错误**





### ReferenceError 对象

reference，引用。

ReferenceError 对象是引用一个不存在的变量时发生的错误。





### RangeError 对象

一个值超出有效范围时发生的错误。





### TypeError 对象

变量或参数不是预期类型时发生的错误。





### URIError 对象

URI 相关函数的参数不正确时抛出的错误，主要涉及`encodeURI()`、`decodeURI()`、`encodeURIComponent()`、`decodeURIComponent()`、`escape()`和`unescape()`这六个函数。



>URI 与 URL
>
>URI，统一资源标识符，是一个用于标识某一互联网资源名称的字符串。
>
>URL，统一资源定位符，是URI的子集，是URI的一种，它标识一个互联网资源，并制定对其操作或获取该资源的方法。
>
>Web上地址的基本形式是URI，它有两种形式：URL和URN。





### EvalError 对象

eval 函数没有被正确执行时，会抛出 EvalError 错误。(基本不再使用)



### 总结

以上6种派生错误，连同原始的Error对象，都是**构造函数**。

开发者可以手动生成错误对象的实例，这些构造函数都接受一个参数，代表错误提示信息message







## 自定义错误

除了JavaScript提供的7种错误对象，还可以定义自己的错误对象。

```js
function UserError(message) {
  this.message = message || '默认信息';
  this.name = 'UserError';
}

UserError.prototype = new Error();//继承Error对象
UserError.prototype.constructor = UserError;//constructor指回UserError函数
```







## throw 语句

`throw` 语句的作用是手动中断程序执行，抛出一个错误。

```js
      //JS程序遇到throw语句就会停止执行
      //throw 语句 抛出默认错误
      throw new Error();
      console.log(1); //不会再执行

      //throw 语句 抛出自定义错误
      throw new UserError();

			//throw 语句可以抛出任何类型的值
      throw 66; //uncaught 66
```

JavaScript 引擎遇到`throw`语句，程序就中止了。





### try...catch 结构

一旦发生错误，程序就中止执行了。

可以使用try...catch结构，对错误进行处理，选择是否继续执行。

```js
try { //try代码块抛出错误，JS引擎会立即把代码的执行转到catch代码块，或者说错误被catch代码块捕获
  throw new Error('出错了');
} catch(err){//catch接受try代码块抛出的值作为参数
  console.log(err.name + ':' + err.message);
  console.log(err.stack);
}
```

如果要处理一些可能会报错的代码，就可以放到try...catch代码块之中：

```js
try {
  f(); //如果f函数执行时出现错误，就会进行catch代码块，对错误进行处理
} catch(err) {
  //处理错误
}
//catch代码块捕获错误后，程序不会中断，会按照正常流程继续执行下去
```

嵌套try...catch

catch代码块之中，还可以再抛出错误：

```js
var n = 100;

try {
  throw n;
} catch (e) {
  if (e <= 50) {
    // ...
  } else {
    throw e;
  }
}
// Uncaught 100

//判断错误类型，进行不同的处理：
try {
  foo.bar();
} catch (e) {
  if (e instanceof EvalError) {
    console.log(e.name + ": " + e.message);
  } else if (e instanceof RangeError) {
    console.log(e.name + ": " + e.message);
  }
  // ...
}
```





### finally 代码块

try...catch 结构允许在最后添加一个finally代码块，表示不管是否出现错误，都必须在最后运行的语句。









# Proxy 和 Reflect 代理与反射



## Proxy

**Proxy** 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。



Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，属于一种元编程（对编程语言进行编程）。



### 创建代理

```js
let proxy = new Proxy(target, handler);
//target 参数表示所要拦截的目标对象
//handler 参数也是一个对象，用来定制拦截行为
```

```js
      //创建代理
      let proxy = new Proxy(
        {},
        {
          get: function (target, propKey) {
            return 35;
          },
        }
      );
      //访问任何属性都将得到35
      console.log(proxy.time); //35
      console.log(proxy.name); //35
      console.log(proxy.title); //35
```



### Proxy 实例的方法

- get()
- set()
- apply()
- has()
- construct()
- deleteProperty()
- defineProperty()
- getOwnPropertyDescriptor()
- getPrototypeOf()
- isExtensible()
- ownKeys()
- preventExtensions()
- setPrototypeOf()

#### get()

用于拦截某个属性的读取操作，可以接受三个参数（目标对象、属性名、proxy实例本身）

```js
      //案例2 利用拦截器设置访问不存在的属性的返回值
      let obj = { name: "Fei" };
      let proxy2 = new Proxy(obj, {
        //创建拦截器
        get: function (target, propKey) {
          //设置get，用于设置拦截器读取属性时的操作
          if (propKey in target) {
            return target[propKey];
          } else {
            throw new ReferenceError("属性名为" + propKey + "的属性不存在");
          }
        },
      });
      console.log(obj.name); //Fei
      console.log(obj.age); //undefined
      //使用拦截器防伪
      console.log(proxy2.name); //Fei
      //   console.log(proxy2.age); //抛出错误
      //get方法可以继承
      let obj1 = Object.create(proxy2);
      //   console.log(obj1.age); //已经继承了拦截器的属性
```





#### set()

set方法用来拦截某个属性的赋值操作，可以接受四个参数，依次为：

目标对象、属性名、属性值、proxy实例本身。















### Proxy.revocable()

`Proxy.revocable()`方法返回一个可取消的 Proxy 实例。











## Reflect

### 概述

reflect 对象与 proxy 对象一样，也是ES6位了操作对象而提供的新API。

Reflect 的设计目的：

- 将 Object对象的一些明显属于语言内部的方法，放到 Reflect 对象上。
- 修改某些Object方法的返回结果，让其变得更合理。
- 让Object操作都变成函数行为。
- Reflect对象的方法与Proxy对象的方法一一对应。



### Reflect的静态方法

- Reflect.apply(target, thisArg, args)
- Reflect.construct(target, args)
- Reflect.get(target, name, receiver)
- Reflect.set(target, name, value, receiver)
- Reflect.defineProperty(target, name, desc)
- Reflect.deleteProperty(target, name)
- Reflect.has(target, name)
- Reflect.ownKeys(target)
- Reflect.isExtensible(target)
- Reflect.preventExtensions(target)
- Reflect.getOwnPropertyDescriptor(target, name)
- Reflect.getPrototypeOf(target)
- Reflect.setPrototypeOf(target, prototype)



















# Promise

解决用回调函数一步一步嵌套导致的回调地狱问题。

JavaScript中存在很多异步操作，promise将异步操作队列化，按照期望的顺序执行，返回符合预期的结果。

**promise链**，通过链式调用将结果一步一步传递下去以达到使用目的。

如果 `.then`（或 `catch/finally` 都可以）处理程序（handler）返回一个 promise，那么链的其余部分将会等待，直到它状态变为 settled。当它被 settled 后，其 result（或 error）将被进一步传递下去。

```js
 //写一个图片请求的promise
      let imgPromise = (url) => {
        return new Promise((resolve, reject) => {
          let img = document.createElement("img");
          img.src = url;
          //成功时 条件是当图片加载事件发生时
          img.onload = () => resolve(img);
          //失败时 条件是图片加载失败
          img.onerror = () => reject(new Error(`图片错误：${url}`));
        });
      };

      //然后开始写promise settled后的处理函数
      let imgUrl = "./images/banner1.jpg";
      imgPromise(imgUrl).then(
        (img) => document.body.appendChild(img),
        (error) => (document.body.innerHTML = error)
      );
      // .catch((error) => alert(`Error: ${error.message}`));
      //错误处理函数可以写在then 的第二个参数里，也可以用catch
```





## 使用promise进行错误处理

捕获所有 error 的最简单的方法是，将 `.catch` 附加到链的末尾

```js
fetch('/article/promise-chaining/user.json')
  .then(response => response.json())
  .then(user => fetch(`https://api.github.com/users/${user.name}`))
  .then(response => response.json())
  .then(githubUser => new Promise((resolve, reject) => {
    let img = document.createElement('img');
    img.src = githubUser.avatar_url;
    img.className = "promise-avatar-example";
    document.body.append(img);

    setTimeout(() => {
      img.remove();
      resolve(githubUser);
    }, 3000);
  }))
  .catch(error => alert(error.message));
//如果上述任意一个 promise 被 reject（网络问题或者无效的 json 或其他），.catch 就会捕获它。

```



### 隐式 try...catch

在浏览器中，我们可以使用 `unhandledrejection` 事件来捕获这类 error.

- `.catch` 处理 promise 中的各种 error：在 `reject()` 调用中的，或者在处理程序（handler）中抛出的（thrown）error。
- 我们应该将 `.catch` 准确地放到我们想要处理 error，并知道如何处理这些 error 的地方。处理程序应该分析 error（可以自定义 error 类来帮助分析）并再次抛出未知的 error（可能它们是编程错误）。
- 如果没有办法从 error 中恢复的话，不使用 `.catch` 也可以。
- 在任何情况下我们都应该有 `unhandledrejection` 事件处理程序（用于浏览器，以及其他环境的模拟），以跟踪未处理的 error 并告知用户（可能还有我们的服务器）有关信息，以使我们的应用程序永远不会“死掉”。





## Promise API

`Promise` 类有 5 种静态方法：

1. `Promise.all(promises)` —— 等待所有 promise 都 resolve 时，返回存放它们结果的数组。如果给定的任意一个 promise 为 reject，那么它就会变成 `Promise.all` 的 error，所有其他 promise 的结果都会被忽略。

2. ```
   Promise.allSettled(promises)
   ```

   （ES2020 新增方法）—— 等待所有 promise 都 settle 时，并以包含以下内容的对象数组的形式返回它们的结果：

   - `status`: `"fulfilled"` 或 `"rejected"`
   - `value`（如果 fulfilled）或 `reason`（如果 rejected）。

3. `Promise.race(promises)` —— 等待第一个 settle 的 promise，并将其 result/error 作为结果。

4. `Promise.resolve(value)` —— 使用给定 value 创建一个 resolved 的 promise。

5. `Promise.reject(error)` —— 使用给定 error 创建一个 rejected 的 promise。





### Promise.all()

用于将多个Promise实例，包装成一个新的Promise实例。

```js
const p = Promise.all([p1, p2, p3]);
//Primise.all() 可以接收一个数组作为参数
//p1, p2, p3都是Promise实例，如果不是，就会先调用Promise.resolve()，将参数转为Promise实例，再进一步处理
//Promise.all()的参数也可以不是数组，但是必须具有Iterator接口，且返回的每个成员都是Promise实例
```

`p`的状态由`p1`、`p2`、`p3`决定，分成两种情况。

（1）只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个**数组**，传递给`p`的回调函数。

（2）只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时**第一个**被`reject`的实例的返回值，会传递给`p`的回调函数。

```js
      //Promise.all()
      //用于将多个Promise实例包装成一个Promise实例
      //这个最终的Promise实例的状态有2种情况：
      //1. 参数里所有的Promise实例的状态都变成了fullfilled，那么该实例的状态就变成fullfilled，并将所有Promise实例的返回值组成一个数组传递给总实例的回调函数
      //2. 只要有一个的状态是rejected，这个总的状态就变成rejected，并返回第一个被rejected的实例的返回值传递给总的回调函数
      //---------
      //生成一个Promise对象的数组
      const promises = [2, 3, 4, 5, 6, 12].map((id) => {
        return getJSON("/post/" + id + ".json"); //jQuery?
      });
      Promise.all(promises) //只有参数里这6个实例的状态都变成fulfilled或者其中一个rejected，才会调用后面的回调函数
        .then((posts) => {})
        .catch((reason) => console.log(reason));
```





### Promise.race()

`Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```JS
const p = Promise.race([p1, p2, p3]);
```

上面代码中，只要`p1`、`p2`、`p3`之中有一个实例**率先改变状态**，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数。

```JS
      const p = Promise.race([
        fetch("/resource-that-may-take-a-while"),
        new Promise(function (resolve, reject) {
          setTimeout(() => reject(new Error("request timeout")), 5000);
        }),
      ]);

      p.then(console.log).catch(console.error);
//上面代码中，如果 5 秒之内fetch方法无法返回结果，变量p的状态就会变为rejected，从而触发catch方法指定的回调函数。
```





### Promise.alSettled()

`Promise.allSettled()`方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例。只有等到所有这些参数实例**都返回**结果，不管是`fulfilled`还是`rejected`，包装实例才会结束。该方法由 [ES2020](https://github.com/tc39/proposal-promise-allSettled) 引入。

```js
const promises = [
  fetch('/api-1'),
  fetch('/api-2'),
  fetch('/api-3'),
];

await Promise.allSettled(promises);
removeLoadingIndicator();//移除滚轮图标
//上面代码对服务器发出三个请求，等到三个请求都结束，不管请求成功还是失败，加载的滚动图标就会消失。
```





### Promise.any()



### Promise.resolve()





### Promise.reject()









# Generator 函数

## 简介

Generator 函数是一个状态机，封装了多个内部状态；还是一个遍历器对象生成函数，可以一次遍历Generator函数内部的每一个状态。

```js
      //Generator函数
      //语法：
      //function*
      //yeild 表示式，定义不同的内部状态（状态是什么？）
      //调用 和普通函数一样() 但不会执行 而会返回一个内部状态的指针
      //next()
      //遇到yeild 停止执行
      //next() 继续执行
      //（分段执行）
      //----------
      //定义生成函数
      function* helloWorldGenerator() {
        yield "hello";
        yield "world";
        return "the end";
      }
      //调用
      let sayHello = helloWorldGenerator();
      //调用next()，执行下一步
      console.log(sayHello.next());
      console.log(sayHello.next());
      console.log(sayHello.next());
      console.log(sayHello.next());
      //{value: "hello", done: false}
      //{value: "world", done: false}
      //{value: "the end", done: true}
      //{value: undefined, done: true}
```

总结：调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。以后，每次调用遍历器对象的next方法，就会返回一个有着value和done两个属性的对象。**value**属性表示当前的**内部状态的值**，是**yield表达式后面那个表达式的值**；done属性是一个布尔值，表示是否遍历结束。





**yeild 与 return**

相似之处在于，都能返回紧跟在语句后面的那个表达式的值。区别在于每次遇到`yield`，函数暂停执行，下一次再从该位置继续向后执行，而`return`语句不具备位置记忆的功能。一个函数里面，只能执行一次（或者说一个）`return`语句，但是可以执行多次（或者说多个）`yield`表达式。正常函数只能返回一个值，因为只能执行一次`return`；Generator 函数可以返回一系列的值，因为可以有任意多个`yield`。



**yeild表达式只能用在Generator函数里面，用在其他地方会报错。**

yeild 表达式如果用在另一个表达式之中，必须放在**圆括号**里面。





## next 方法的参数

**yeild表达式本身没有返回值(undefined)**，next方法可以带一个参数，该参数就会被当作上一个 yeild 表达式的返回值。

```js
      function* f() {
        for (var i = 0; true; i++) {
          var reset = yield i; //yeild表达式本身没有返回值(undefined),所以这里reset默认就是undefined
          if (reset) {
            //所以这里if默认会是false，直到reset的值为真
            i = -1;
          }
        }
      }

      var g = f();

      console.log(g.next()); // { value: 0, done: false }
      console.log(g.next()); // { value: 1, done: false }
      console.log(g.next()); // { value: 2, done: false }
      console.log(g.next()); // { value: 3, done: false }
      //这里给next方法传了一个参数true，这个参数会被当作上一个yeild表达式的返回值
      //然后函数内部的reset的值就变成了true，执行了if语句内的代码，i=-1,下一轮循环就开始从-1递增
      console.log(g.next(true)); // { value: 0, done: false }
```

通过next方法的参数，就可以在Generator函数开始运行之后，继续向函数体内部注入值。可以在Generator函数运行的不同阶段，从外部向内部注入不同的值，从而调整函数行为。



```js
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
```

注意，由于`next`方法的参数表示上一个`yield`表达式的返回值，所以在第一次使用`next`方法时，传递参数是无效的。





## for...of

`for...of`循环可以自动遍历 Generator 函数运行时生成的`Iterator`对象，且此时**不再需要调用**`next`方法。







# Async/await

async 函数是什么？一句话，它就是 Generator 函数的语法糖。

async 函数返回一个Promise对象，

```js
      //async 函数
      async function chainAnimationsAsync(elem, animations) {
        let ret = null;
        try {
          for (let anim of animations) {
            ret = await anim(elem);
          }
        } catch (e) {
          //忽略错误，继续执行
        }
        return ret;
      }
```















# 模块设计 Module



## 了解

模块化，拆分，分工合作

**模块**：一个一个的局部作用域的代码块

以前模拟模块使用立即执行函数，避免出现全局变量冲突，如果需要使用全局变量，就用window.变量 暴露出去，引入JS时要注意先后顺序



**模块系统**

可以解决：

- 模块化的问题
- 消除全局变量
- 管理加载顺序



**Module 的加载**

引入JS时只用引入最末端的JS文件，加上type="module"加载模块

也不再需要使用立即执行函数，也可以方便的管理顺序，自己会沿着import的路径取查找

`<script src="./js/index.js" type="module"></script`



## Module的导入和导出 

**模块的导出和导入：**

- 一个模块的导出可以被其他模块导入并访问
- 没有导出，也可以将其导入，被导入的代码将会执行一遍，也仅会执行一遍





**Module的两种导出和导入方式**

- export default 导出和对应的import导入

  - `export default age;`
    - 一个模块只能有一个export default，用export default导出多个会报错
  - `import age from './module.js';`

- export 导出和对应的 import 导入

  - 基本用法

    - export 后面跟的是 声明或者语句
    - `export const age = 18;` 或者 `export {age};`
    - `import {age} from './module.js'; //这里的age不能随意命名，要加{}像解构一样`

  - 多个导出

    - `export {age, fn, className};`
    - `import {age, fn, className} from './module.js';`

  - 导出导入时起别名

    - `export {age as uage, fn as func};` 用 as 起别名
    - `import {uage, func as MyFn} from './module.js';` 导入时依然可以用as起别名

  - 整体导入

    - `import * as obj from './module.js';` *是通配符的意思

  - 同时导入

    - `import age, {uage, fn, className} from './module.js';` 同时导入 export default 和 export

    





## Module 的应用

默认参数、操控、常量等都可以作为单独的模块拆分出去，代码更灵活。







Module的注意事项

1. 模块顶层this的指向

   - 指向 `undefined`

2. import关键字和import()函数

   - import 命令具有**提升**效果，会提升到整个模块的头部，率先执行，import执行的时候，所有的代码还没执行

   - import() 可以按条件导入(动态导入模块)(不能滥用)

     - ```js
       if(PC) {
         import('pc.js').then().catch();
       } else if(Mobile) {
         import('mobile.js').then().catch();
       }
       //import()将返回一个 promise
       ```

3. 导入导出的复合写法（不推荐）

   - ```js
     //在引入文件直接用一句语句来完成导入导出，但阅读性差
     export {age} from './module.js';
     
     //相当于
     import {age} from './module.js';
     export {age} from './module.js';
     ```

     











# Babel 与 Webpack

## Babel 



**Babel 介绍**

- Babel 是 JavaScript 的编译器，用来将ES6的代码，转换成ES6之前的代码。
- Babel 本身可以编译ES6的大部分语法，比如let, const， 箭头函数，类，但是对于ES6新增的API，比如 Set, Map, Promise等全局对象，以及一些定义在全局对象上的方法等都不能直接编译，需要借助其他的模块
- Babel 一般需要配合 Webpack 来编译模块语法。



**Babel 的使用方式**

- https://babeljs.io/setup



**使用 Babel 前的准备工作** 

- 安装 Node.js
- 初始化项目
  - `npm init`
- 安装Babel需要的包
  - `npm install --save-dev @babel/core @babel/cli`
  - `npm install --save-dev @babel/core@7.11.0 @babel/cli@7.10.5` 安装指定版本的包



**使用 Babel 编译 ES6 代码**

- 在`package.json` 里的`script`区域添加一条命令：`"build": "babel src -d lib"` (意思就是将src里的文件编译并输出到lib文件夹里)

- 配置`.babelrc`, 设置转码规则

  - 配置文件.babelrc(所有babel工具和模块的使用，都必须先写好此文件)

    - ```javascript
      {
        "presets": [// 设置转码规则
          "@babel/env",
        ], 
        "plugins": [] 
      }
      ```

- 运行
  - `npm run build`







## Webpack 入门

webpack 是**静态模块打包器**，当webpack处理应用程序时，会将所有这些模块打包成一个或多个文件。

webpack 可以处理js/css/图片/图标字体等各种模块。

webpack只能处理存储在本地的js/css/图片/图标字体等**静态**模块。

动态的内容，webpack没办法处理。





webpack 初体验：

- 初始化项目

  - npm init

- 安装 webpack 需要的包

  - `npm install --save-dev webpack-cli webpack`

- 配置 webpack

  - 根目录创建`webpack.config.js`配置文件

  - ```js
    //配置
    const path = require('path');
    
    module.exports = {
      entry: './src/index.js',
      output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
      }
    };
    ```

- 打包并测试

  - 在html中引入编译过的js文件进行测试
  - `<script src="./dist/bundle.js"></script>` 



## Webpack 的核心概念

**entry**

- 入口文件设置

- `entry: './src/index.js'` 或者

- ```js
  //多入口
  entry: 
    main: './src/index.js',
    login: './src/login.js',
  }
  ```

**output**

- 输出文件设置

  - ```js
    output: {
      path: path.resolve(__dirname, 'dist'),
        filename: '[name].js' 
      //有多个文件需要输出，会直接去拿每个文件的名字
     
    }
    ```

    



**loader**

- loader 可以让 webpack能够去处理那些非JS文件的模块

- babel-loader
  - babel是babel，webpack是wepack，两者如果要联合使用，就要使用babel-loader
  - 安装babel-loader
    - `npm install --save-dev babel-loader`
  - 安装babel
    - `npm install --save-dev @babel/core @babel/preset-env`
  - 配置babel-loader 
    - https://webpack.js.org/concepts/loaders/
  - 引入 core-js
    - 编译新增的API
    - `npm install --save-dev core-js`
    - `import "core-js/stable";`
  - 编译并测试
    - `npm run build`



**plugins**

- plugins，插件，可以用来处理各种复杂的任务。loader用于帮助webpack处理各种模块，而插件则可以用于执行范围更广的任务。
- 插件很多，根据项目需求，查看文档学习---
- html-webpack-plugin
  - 安装
    - `npm install --save-dev html-webpack-plugin`
  - 配置插件
    - https://www.webpackjs.com/concepts/plugins/



webpack-dev-server

- 每次要编译代码时，手动运行 `npm run build` 就会变得很麻烦,解决：

- webpack-dev-server 能够用于快速开发应用程序。

- https://www.webpackjs.com/configuration/dev-server/



## Webpack 的应用















# 任务管理







# DOM



# 空间坐标



# 事件



#### 网络请求

