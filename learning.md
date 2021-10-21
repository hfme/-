- ES6
- Vue底层原理
- Vue-Router





## Vue 

##### Vue 实例

- **Vue实例**被创建时，它将**data对象**中所有属性加入到Vue的**响应式**系统中
- 只有当实例被创建时就已经存在于 `data` 中的 property 才是**响应式**的

```js
// 数据 property
var data = {a: 1}; //实例创建时已存在的属性
var vm = new Vue({
    data: data
});

vm.a === data.a; //true

vm.b = 2; //实例创建后添加的data属性不会有响应


```

- Vue 实例还暴露了一些有用的实例 property 与方法。它们都有前缀 `$`

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data; //true
vm.$el = document.getElementById('example')
vm.watch....
```



- v-if 条件渲染时可以实用key来解除复用
- v-for
  - 遍历数组
    - (item, index) in arr
  - 遍历对象
    - (value, name, index ) in object
  - key 的作用：
    - `key` 的特殊 attribute 主要用在 Vue 的虚拟 DOM 算法，在新旧 nodes 对比时辨识 VNodes
    -  跟踪每个节点的身份，从而重用和重新排序现有元素
    - 使用字符串或数值类型的值
    - 可以使用template标签包裹使用v-for来循环渲染包含多个元素的内容



##### 事件相关

- 使用$event传入方法中来访问原始的**DOM事件**

```js
```









##### 作用域插槽

- 让插槽内容访问子组件中的数据

```html
// 子组件中定义插槽
<slot name="childCom" v-bind:user="user">{{user.lastName}}</slot>

// 父组件使用
<template v-slot:childCom="slotProps">
	<span>{{slotProps.user.firstName}}</span>
</template>

// 更简洁写法 解构
<template v-slot:childCom="{user}">
	<span>{{user.firstName}}</span>
</template>

// 解构时可以重命名
<template v-slot:childCom="{user: userName}">
	<span>{{userName.firstName}}</span>
</template>

// 解构时还可以定义备用值
<template v-slot:childCom="{user = { firstName: 'John' }}">
	<span>{{user.firstName}}</span>
</template>
```





##### 计算属性

- 基于其响应式依赖进行缓存

```js
computed: {
    fullName: {
        get: function(){
            return this.firstName + ' ' + this.lastName
        },
        set: function(newVal){
            let name = newVal.split(' ')
            this.firsetName = name[0]
            this.lastName = name[name.length - 1]
        }
    }
}
```



##### 侦听器

- 需要异步时，侦听数据，响应变化

```js
watch: {
    //侦听question的变化，如果发生变化，函数就会运行
    question: function(newVal, oldVal){
        this.getDataList()
    }
}
```



##### axios请求

- 使用async/await处理异步请求
- 使用解构简化代码

```js
async getDataList(){
    const {data } = await getTableData(this.searchOptions);
     if (data.code === 200) this.tableData = data.list;
}
```



##### 组件

- 命名规范

  - 在html中使用时：字母全小写且必须包含一个连字符

- 局部注册组件模块

- 基础常用小组件使用 require.context 在main.js中全局注册，避免频繁引入

- 使用prop给子组件传递数据

  - 静态或动态prop
  - 单向数据流

- 监听子组件事件

  - 父组件中定义方法
  - 子组件使用$emit触发事件，同时可以传递参数

- 通过插槽分发内容

- 动态组件与异步组件

  - 使用keep-alive缓存组件状态
  - 异步组件：用工厂函数定义组件，在需要时再加载组件模块

- 访问元素和组件

  - `this.$root.property` (适用非常小型的有少量组件的应用)

- 访问父级组件实例

  - `this.$parent.property` (会使得应用更难调试和理解)

- 访问子组件实例或子元素

  - 在子组件html标签上加 ref
  - this.$refs.focus()....
  - `$refs` 只会在组件渲染完成之后生效, 避免在模板或计算属性中访问 `$refs`

- 依赖注入

  - provide/inject

  - 在父级组件中指定需要提供给后代组件的数据或方法

  - 在需要使用的后代组件中，使用inject接收

  - 可以把依赖注入看作一部分“大范围有效的 prop”

  - ```js
    // 父级组件
    provide: function () {
      return {
        getMap: this.getMap
      }
    }
    
    //后代组件
    inject: ['getMap']
    ```



##### 可复用性

###### 混入

- 定义mixins对象
- 在需要使用的地方引入，并注入mixins:  `mixins: [myMixin]`
- 同名钩子函数会合并为一个数组，都会被调用，混入对象的钩子会先执行
- 值为对象的选项，如methods, 将被合并为同一个对象，键名冲突时取组件的值
- 数据对象data会进行递归合并，冲突时取组件数据
- 可以自定义选项合并策略

###### 自定义指令

- 局部指令 directives: {}
- 全局指令 Vue.directive('指令名', {})
- 钩子函数 
- 钩子函数参数 el binding vnode oldVnode

###### 过滤器

###### 渲染函数&JSX

- 虚拟DOM 对由 Vue 组件树建立起来的整个 VNode 树的称呼
- 

```js
// template 模板
<h1>{{ blogTitle }}</h1>

// render函数
render: function (createElement) {
  return createElement('h1', this.blogTitle)
}
```



###### 插件













#### ES6

##### 解构

- 取值

```js
// 解构的对象不能是 undefined, null, 最好给结构对象一个默认值
const {a, b, c: c1} = obj || {};
```





##### 扩展运算符

- 数组或对象合并

```js
// 数组合并
// concat 不能去重
const arr1 = [1, 2, 3];
const arr2 = [3, 4, 5];
const arr = [...new Set([...arr1, ...arr2])]; //利用set去重


// 对象合并
// Object.assign
```





##### 数组搜索

- filter 模糊搜索
- find 精确搜索，搜到即停





##### 可选链操作符

- 直接访问对象的深层属性

```js
const buzz = obj?.person?.job?.buzz;
```





##### async/await

- 更简洁的方式写出基于[`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)的异步行为

```js
function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('resolved');
    }, 2000);
  });
}

async function asyncCall() {
  console.log('calling');
  const result = await resolveAfter2Seconds();
  console.log(result);
  // expected output: "resolved"
}

asyncCall();
```





##### 引用类型

- 数组，可以使用 **slice**, **concat**, **扩展语法**实现复制数组 （浅拷贝）
- 对象， 扩展语法，Object.assign, （浅拷贝）  JSON.parse(JSON.stringify()) 深拷贝 

```js
      // 首先我们有一个数组
      const players = ["Wes", "Sarah", "Ryan", "Poppy"];

      const arr1 = players;
      arr1[1] = "Fei";

      // 使用slice, concat, 扩展语法 或者 Array.from
      const arr2 = players.slice();
      const arr3 = [].concat(players);
      const arr4 = [...players];
      const arr5 = Array.from(players);
      arr2[1] = "Neo";
      arr3[3] = "John";
      arr4[2] = "Harry";
      arr5[0] = "Puppy";
      console.log(arr1, arr2, arr3, arr4, arr5);
```

