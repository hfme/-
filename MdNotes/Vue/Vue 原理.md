# Vue 原理

## VDOM

用JS模拟DOM结构

<img src="/Users/neofei/Fei/Frontend/笔记/images/image-20210804091223761.png" alt="image-20210804091223761" style="zoom:50%;" />



```js
// VNode
{
  tag: 'div',
  props: {
    className: 'container',
    id: 'div1'
  },
  children: [
    {
      tag: 'p',
      children: 'vdom'
    },
    {
      tag: 'ul',
      props: {
        style: 'font-size: 20px',
      },
      children: [
        {
          tag: 'li',
          children: 'a'
        },
      ]
    },
  ]
}
```





### snabddom

> 简洁强大的vdom库，易学易用
>
> Vue中的VDOM和diff就是参考它
>
> github： https://github.com/snabbdom/snabbdom



diff 算法

通过比较渲染前后的数据，只有当数据有变动才会发生真正的渲染

