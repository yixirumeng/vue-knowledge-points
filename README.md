## Vue知识点总结

[1、Vue动态参数绑定](#Vue动态参数绑定)


1. ### Vue动态参数绑定

vue在2.6版本后提供了动态参数绑定功能，其使用方式为：
```
<button @[attribute]="doSomething">按钮</button>

data(){
    return {
        attribute: 'click'
    }
},
method: {
    doSomething(){
        console.log('button')
    }
}

```
不只可以用于事件，用于`v-bind`也是可以。动态参数正常应为字符串，如果为`null`，则相当于移除绑定。

动态参数相当于为`html`绑定属性，所以要符合`html`属性绑定规范，比如带着空格、引号等是无效的，而大写字母则会被转换为小写。如果动态参数需要通过运算处理，则需要放到计算属性中处理过后再绑定到元素上。
