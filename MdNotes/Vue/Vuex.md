# Vuex 简介

## 介绍

Vuex是一个专为Vue.js应用程序开发的状态管理模式，采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

Vuex 主要应用于Vue中**管理数据状态**的库。

通过创建一个集中的数据存储，供程序中所有组件访问。





![image-20210507165700287](/Users/neofei/Fei/Frontend/笔记/MdNotes/images/image-20210507165700287.png)

1. 创建Vuex对象

2. 在Vuex里的**state**，添加数据进行共享

   - 使用：在父组件中存入 store: store
   - this.$store.state.xxx  父组件和子组件直接使用共享数据

3. 在Vuex的**getters**里，定义Vuex的计算属性

   - 使用：this.$store.getters.xxx

4. 在Vuex里的**mutations**里，添加方法，进行调用和传参

   - 使用：在组件中用 this.$store.commit('函数名')调用mutations里的函数

   

   案例

   ```javascript
   const store = new Vuex({
     //state, 保存共享数据，相当于组件中的data
     state: {
       message: '这是共享的数据',
       count: 0,
     },
     
     //mutations, 里面存放 修改共享数据的方法
     mutations: {
       addCount(state) {
         state.count++;
       },
       subCount(state) {
         state.count--;
       }
     },
     
     //Vuex getters属性，与实例的computed计算属性类似，用于定义计算属性，会将计算结果缓存起来
     getters: {
       reversMsg(state) {
         return state.message.split('').reverse().join('')
       }
     }
   });
   ```



课程里的项目介绍：

排行榜



## State

存储数据

## Getters

获取数据

辅助函数：

mapGetters

`mapGetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性：

```javascript
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```



## Mutations

更改Vuex的store中的状态，类似于事件。

触发事件



## Actions

类似于mutations，不同在于 ：

- action提交的是mutation，而不是直接变更状态 ？？
- action可以包含任意异步操作 ？？

参数 context

dispatch 分发   

传参

payload 接收参数



案例

```javascript
const store = new Vuex.Store({
  state:{
    persons: [{...}],
  },
  getters:{},
  mutations: {
    subGap(state, payload){  //payload是actions传递过来的参数
      let newPersons = state.persons.map((person)=>{
          person.unit -=payload;//使用参数
      });
    }
  },
  actions: {
    subGapAction(context){ //context参数其实就是this.$state
      setTimeout(function(){
        context.commit('subGap', payload); //将函数提交到mutation， 并通过payload传递参数
      }, 2000);
    }
  }
});


//组件里使用store里的数据或方法
<button @click="subGap(200)"></button>

export default {
  methods: {
    subGap(subAmount) { //接收button传递过来的参数
      //这里使用dispatch 分发 调用actions里的函数
      //Action 通过 store.dispatch 方法触发：
      return this.$store.dispatch('subGapAction', subAmount); //将参数传递给actions里的函数
    }
  }
}
```

辅助函数：

mapActions



在组件中分发Action









## Modules

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

```javascript
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```





项目笔记：







复习：

使用Vue传递数据的方式：

**子组件使用父组件的data里的数据**:

1. 要通过v-bind绑定传递的数据： 
   - v-bind：自定义接收名称 = "要传递的数据"
2. 在子组件中通过props接收数据：
   -  props: ["上面自定义的接收名称"]
3. 在子组件里使用模版语法使用props里接收的数据
   - 如： <p>{{自定义的接收名称}}</p>





**子组件使用父组件的方法**（父组件传递）：

1. 在父组件template元素内的子组件标签上，通过**v-on传递方法**：

   - v-on:接收方法名称 = “要传递的方法”

2. 在子组件methods中使用接收方法定义一个方法，在方法内使用 **this.$emit('方法名', 参数)** 来触发传递过来的方法，同时可以利用第2个参数来给父组件方法传递参数

   

