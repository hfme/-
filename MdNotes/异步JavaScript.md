# 异步JavaScript

asynchronous JavaScript

问题：

异步为什么重要？

怎样使用异步来有效处理潜在的阻塞操作，比如从服务器上获取资源。





# 通用异步编程概念

理解异步编程的基本概念，以及异步编程在浏览器和JavaScript里面的表现。

## 异步介绍

异步编程的出发点，减少等待



## 阻塞





## 线程

一个线程是一个基本的处理过程，程序用它来完成任务。每个线程一次只能执行一个任务。

每个任务顺序执行，只有前面的执行结束，后面的才会开始执行。

```
Task A --> Task B --> Task C
```

**多线程**

现代计算机都有多个内核，可以同时执行多个任务，支持多线程的编程语言可以使用计算机的多个内核，可以同时完成多个任务。

```
Thread 1: Task A --> Task B
Thread 2: Task C --> Task D
```



## 异步代码

web workers

```
Main thread: Task A --> Task C
Worker thread: Expensive task B
```

web workers相当有用，但是他们确实也有局限。主要的一个问题是他们不能访问 [DOM](https://developer.mozilla.org/zh-CN/docs/Glossary/DOM) — 不能让一个worker直接更新UI。我们不能在worker里面渲染1百万个蓝色圆圈，它基本上只能做算数的苦活。

其次，虽然在worker里面运行的代码不会产生阻塞，但是基本上还是同步的。当一个函数依赖于几个在它之前运行的过程的结果，这就会成为问题。





# 介绍异步JavaScript

熟悉什么是异步JavaScript，与同步JavaScript 的区别，以及使用场合。



异步编程风格：

- 老派的 callbacks
- 新派 promise



## 异步callback

异步callback是把回调函数作为参数传递给另一个函数（在后台执行的其他函数）在某个地方异步执行，当后台运行的代码介绍，就调用callbacks函数，返回结果。

案例1:

```javascript
function loadAsset(url, type, callback) {
  let xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.responseType = type;

  xhr.onload = function() {
    callback(xhr.response);//这里callback就相当于 displayImage函数，这里把xhr.response作为参数回传给了 displayImage函数
  };

  xhr.send();
}

function displayImage(blob) {
  let objectURL = URL.createObjectURL(blob);

  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}

loadAsset('coffee.jpg', 'blob', displayImage);// 这里的displayImage函数作为loadAsset函数的第三个参数传递了过去，loadAsset函数用XHR获取url资源，在获得资源响应后再把响应作为参数传递给回调函数去处理。
```

回调函数用途广泛 — 他们不仅仅可以用来控制函数的执行顺序和函数之间的数据传递，还可以根据环境的不同，将数据传递给不同的函数，所以对下载好的资源，你可以采用不同的操作来处理，譬如 `processJSON()`, `displayText()`, 等等。



## Promise

Promise是新派的异步代码，现代的web APIs经常用到，如fetch() API，它基本就是一个现代版的、更高效的 XMLHttpRequest.



promise翻译就是承诺，相当于浏览器承诺说“我保证尽快给你答复”





(Promise这样的异步代码在EvenLoop中称为微任务，会被放入事件队列中，会在主线程同步任务处理完之后运行。)





### Promise 与 callback 对比

它们本质上都是一个返回的对象

`Promise`是专门为异步操作而设计的，与旧式回调相比具有许多优点:

- 您可以使用多个then()操作将多个异步操作链接在一起，并将其中一个操作的结果作为输入传递给下一个操作。这种链接方式对回调来说要难得多，会使回调以混乱的“末日金字塔”告终。 (也称为[回调地狱](http://callbackhell.com/))。
- `Promise`总是严格按照它们放置在事件队列中的顺序调用。
- 错误处理要好得多——所有的错误都由块末尾的一个.catch()块处理，而不是在“金字塔”的每一层单独处理。

# 

## 异步代码案例

案例：

```javascript
console.log ('Starting');
let image;

fetch('coffee.jpg').then((response) => {
  console.log('It worked :)')
  return response.blob(); //Blob对象表示一个不可变、原始数据的类文件对象。
}).then((myBlob) => {
  let objectURL = URL.createObjectURL(myBlob);
  image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}).catch((error) => {
  console.log('There has been a problem with your fetch operation: ' + error.message);
});

console.log ('All done!');

//输出结果：
// Starting
// All done!
// It Worked

//第一个log和第三个log会立即输出，但是第二个消息在promise链上被阻塞，直到获取资源后才会显示
//第二个消息在fetch()块中，fetch()块石异步执行的，
```

如果把第3个消息修改：

```javascript
console.log ('Starting');
let image;

fetch('coffee.jpg').then((response) => {
  console.log('It worked :)')
  return response.blob();
}).then((myBlob) => {
  let objectURL = URL.createObjectURL(myBlob);
  image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}).catch((error) => {
  console.log('There has been a problem with your fetch operation: ' + error.message);
});

console.log ('All done! ' + image.src + 'displayed.');
//报错
// 由于中间的异步代码会被加入微任务队列，第1个和第3个消息会立即执行，执行第3个log，此时由于fetch()语句块没有完成，其中的image变量还没有赋值，所以控制台会报错

```

解决：
把第3个log代码异步化

```javascript
console.log ('Starting');
let image;

fetch('coffee.jpg').then((response) => {
  console.log('It worked :)')
  return response.blob();
}).then((myBlob) => {
  let objectURL = URL.createObjectURL(myBlob);
  image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}).then(()=>{
  //把第3个消息链接到image已经赋值完成then()块后面进行异步处理
  console.log ('All done! ' + image.src + 'displayed.');
}).catch((error) => {
  console.log('There has been a problem with your fetch operation: ' + error.message);
});

//Starting
//It worked :)
//All done! blob:https://mdn.github.io/f157eb15-8fed-43cc-8158-650503e140b6 displayed.
```



## 总结

在最基本的形式中，JavaScript是一种同步的、阻塞的、单线程的语言，在这种语言中，一次只能执行一个操作。但**web浏览器定义了函数和API**，允许我们当某些事件发生时不是按照同步方式，而是**异步地调用函数**(比如，时间的推移，用户通过鼠标的交互，或者获取网络数据)。这意味着您的代码可以同时做几件事情，而不需要停止或阻塞主线程。

异步还是同步执行代码，取决于我们要做什么。

有些时候，我们希望事情能够立即加载并发生。例如，当将一些用户定义的样式应用到一个页面时，您希望这些样式能够尽快被应用。

但是，如果我们正在运行一个需要时间的操作，比如查询数据库并使用结果填充模板，那么最好将该操作从主线程中移开使用异步完成任务。随着时间的推移，您将了解何时选择异步技术比选择同步技术更有意义。



# 合作异步JS：超时与间隔

了解异步循环和间隔及其用途。



**setTimeout()** ：在指定时间后执行代码

**setInterval()**： 在固定的时间间隔，重复执行代码

**requestAnimationFrame()**：是setInterval的现代版本，在浏览器下一次重新绘制显示之前执行代码，从而允许动画在适当的帧率下运行

web平台提供了一些延时函数，允许在一段时间后异步执行/重复执行代码



## setTimeout()

案例1

```javascript
let myGreeting = setTimeout(function sayHi(who) {
  alert('Hi,' + who + '!');
}, 2000);
```



```javascript
// 定时执行的函数可以单独声明，这样做的好处：
// 1. 有利于保持代码整洁,特别是函数体较长时
// 2. 其他事件/函数也可以方便地调用此函数

function sayHi(who) { //传递参数
  alert('Hi, ' + who + '!');
}
let myGreeting = setTimeout(sayHi, 2000, 'Fannie'); 
//要给setTimeout中的运行函数传递参数，必须在末尾传递。这里就是给sayHi函数传递了Fannie参数，相当于sayHi('Fannie')
```

清除定时器

```javascript
clearTimeout(myGreeing);
```





## setInterval



案例

```javascript
// 一个时钟

function displayTime() {
   let date = new Date();
   let time = date.toLocaleTimeString();
   document.getElementById('demo').textContent = time;
}

const createClock = setInterval(displayTime, 1000);
```

清除interval

```javascript
clearInterval(myInterval);
```



递归setTimeout() 和 setInterval 区别：

```javascript
//递归setTimeout()
let i = 1;

setTimeout(function run() {
  console.log(i);
  i++;
  setTimeout(run, 100);
}, 100);


//setInterval
let i = 1;
setInterval(function run() {
  console.log(i);
  i++
}, 100);
```

- 递归 `setTimeout()` 保证执行之间的延迟相同，例如在上述情况下为100ms。 代码将运行，然后在它再次运行之前等待100ms，因此无论代码运行多长时间，间隔都是相同的。
- 使用 `setInterval()` 的示例有些不同。 我们选择的间隔包括执行我们想要运行的代码所花费的时间。假设代码需要40毫秒才能运行 - 然后间隔最终只有60毫秒。
- 当递归使用 `setTimeout()` 时，每次迭代都可以在运行下一次迭代之前计算不同的延迟。 换句话说，第二个参数的值可以指定在再次运行代码之前等待的不同时间（以毫秒为单位）。

当你的代码有可能比你分配的时间间隔，花费更长时间运行时，最好使用递归的 `setTimeout()` - 这将使执行之间的时间间隔保持不变，无论代码执行多长时间，你不会得到错误。





**立即超时**

使用0用作setTimeout()的回调函数会立刻执行，但是**在主线程代码运行之后执行**。

案例

```javascript
setTimeout(function() {
  alert('World');
}, 0);

alert('Hello');
```

如果您希望设置一个代码块以便在**所有主线程完成运行后立即运行**，这将很有用。将其放在异步事件循环中，这样它将随后直接运行。





## requestAnimatinFrame()

requestAnimationFrame() 是一个专门的循环函数，旨在浏览器中高效运行动画。

它是针对`setInterval()` 遇到的问题创建的，比如 `setInterval()`并不是针对设备优化的帧率运行，有时会丢帧。还有即使该选项卡不是活动的选项卡或动画滚出页面等问题 。

撤销

```
cancelAnimationFrame(rAF);
```



就是这样-异步循环和间隔的所有要点在一篇文章中介绍了。您会发现这些方法在许多情况下都很有用，但请注意不要过度使用它们！因为它们仍然在主线程上运行，所以繁重的回调（尤其是那些操纵DOM的回调）会在不注意的情况下降低页面的速度。



# 优雅的处理异步操作：Promise

理解并使用学习如何使用Promises

















# 让异步编程简单: async and await















# 正文开始 GO！



ES6中最重要的：

class

promise

 module 





Promise 相关：

迭代器

生成器

promise

async/await



遍历和迭代的区别？

回忆以前使用的各种遍历方式：

- array: for循环、forEach()
- string: for循环
- object: for...in



或者使用 ...

... 扩展运算符，ES6 新增

```javascript
//数组
let arr = [1,2,4,5,6];
console.log(...arr); // 1 2 4 5 6

//对象
let person = {
	uname: "fei",
  age: 30
};
console.log(...person); //{uname: "fei", age: 30}


//把数组中的元素迭代为函数参数
function fn(x, y, z) {}
let args = [1, 2, 3];
fn(...args);
```



# **迭代器接口与for...of**

数据对象的成员可以遍历，是因为该对象实现了**Iterator接口**，（迭代器接口）。

```javascript
//for...of 遍历数组对象
let arr = [2,4,3];
for(let i of arr) {
  console.log(i); //2 4 3
}
```

任何数据只要部署Iterator接口，就可以完成遍历操作。



**迭代器本质**：

是一个指针对象，通过指针对象的`next()`，执行该方法要么返回迭代中的下一项，要么就引起一个Stopiteration异常，以终止迭代。

`next()`返回一个对象，表示当前数据成员的信息。这个对象具有`value`(返回当前位置的成员)和`done`（返回布尔值，表示遍历是否结束）两个属性。



ES6规定，默认的Iterator接口部署在数据结构的原型里的`Symbol(Symbol.iterator): ƒ [Symbol.iterator]()` 上



原生具备Iterator接口的数据结构如下：

Array、Map、Set、String、TypeArray、函数的arguments对象、NodeList 对象



```javascript
//获取一个数据对象的原型
// ES5 使用 xxx.__proto__ 不推荐？
// ES6 使用 Object.getPrototypeOf(xxx)

let str = "tomato";
console.log(Object.getPrototypeOf(str)); //Symbol(Symbol.iterator): ƒ [Symbol.iterator]()
let arr = [1,2,4,];
let iter = arr[Symbol.iterator]();
//返回的是迭代器对象，通过next()方法可以逐步调用
console.log(iter.next()); // {value: 1, done: false}
console.log(iter.next()); // {value: 2, done: false}
console.log(iter.next()); // {value: 4, done: false}
console.log(iter.next()); // {value: undefined, done: true}
```





模拟Iterator接口，可以达到上面一样的效果

```javascript
function makeIterator(arr){
  var index = 0;
  return {
    next() {
      if(index < arr.length) {
        return {
          value: arr[index++], done: false
        }
      }
      // 如果index超出数组长度，表明已经迭代完数组，那么
      return {value: undefined, done: true}
    }
  }
}
let arr = [1,2,4,];
let iter = makeIterator(arr);
console.log(iter.next()); // {value: 1, done: false}
console.log(iter.next()); // {value: 2, done: false}
console.log(iter.next()); // {value: 4, done: false}
console.log(iter.next()); // {value: undefined, done: true}
```

