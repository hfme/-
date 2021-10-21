## TyepeScript

微软开发开源

是一种给JavaScript添加特性的语言扩展，是JavaScript的超集。



安装

npm install typescript -g



环境：

直接node运行.ts的文件是不能运行的，需要

- 转成.js，命令行： tsc 1.demo.ts 会生成一份js，再运行JS（麻烦）
- 安装ts-node,直接用ts-node 1.demo.ts 运行
  - npm install ts-node -g (要全局安装才行？)





静态类型 Static Typing

- 不能再改变数据类型
- 继承了定义的数据类型的属性和方法

自定义静态类型：

- interface Hero {} （接口？）



静态类型的类型

- 基础静态类型
- 对象类型
  - 对象类型
  - 数组类型
  - 类类型
  - 函数类型





type annotation 类型注解

type inference 类型推断



TS工作使用潜规则：

- 如果TS能够自动分析变量类型，就不需要类型注解
- 如果TS无法自动分析变量类型的话，就需要使用类型注解





函数参数和返回类型的注解

返回类型也可以进行注解：

```javascript
function sum(num1: number, num2:number):number { //直接给圆括号后面整个返回类型进行类型注解
	return num1 + num2;
}
const total = sum(1, 2);
```

```javascript
function sayHi():void { //这里的返回类型写void， 因为该函数没有返回值
	console.log("Hi");
}
```

永远执行不完的函数

```javascript
function erroFun():never { //永远也执行不完
	while(true){} 
	console.log("Hi");
}
```









