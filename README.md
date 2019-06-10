## Vue知识点总结

[1、Vue动态参数绑定](#Vue动态参数绑定)

[2、如何使数据不进行响应式更新](#如何使数据不进行响应式更新)

[3、为什么不要在选项属性或回调上使用箭头函数](#为什么不要在选项属性或回调上使用箭头函数)


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
    不只可以用于事件，用于`v-bind`也是可以。动态参数正常应为字符串，如果为`null`，则相当于**移除绑定**。

    动态参数相当于为`html`绑定属性，所以要符合`html`属性绑定规范，比如**带着空格、引号等是无效的**，而**大写字母则会被转换为小写**。**如果动态参数需要通过运算处理，则需要放到计算属性中处理过后再绑定到元素上**。

2. ### 如何使数据不进行响应式更新

    默认在`data`中定义的数据都会进行响应式更新，也就是通过调用`Object.defineproperty`方法的`get`、`set`来检测数据变化。如果某些数据不会改变，而且也防止被改变，则需要阻止数据响应式更新。其方式是调用`Object.freeze`方法来冻结数据，即：

    ```
    const unchangeObj = {title: 'hello'}

    data(){
        return {
            obj: Object.freeze(unchangeObj)
        }
    }
    ```
    这样在使用`title`时，即使对其更改，也会有报错提醒。但是这并不能阻止`obj`的改变，如果阻止任何数据更新，则需要`return`一个冻结的对象。

    其原理是，`Object.freeze`方法将使得对象的属性不能进行修改、删除、新增等操作，也就是将对象的`descriptor`下的`configurable`执为`false`。这样在调用`Object.defineproperty`之前，将不会为数据设置`get`、`set`方法时，使得数据不进行响应式更新。

3. ### 为什么不要在选项属性或回调上使用箭头函数

    ```
    created: () => console.log(this.a)

    methods: {
        doSomething: () => console.log(this.a)
    }
    ```
    因为箭头函数本身没有`this`，`this`会作为变量一直向上级词法作用域查找直到找到为止，这样经常会导致控制台报出错误信息。所以不要在选项属性或回调上使用箭头函数，避免不必要的错误发生。
