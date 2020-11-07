https://www.zhihu.com/question/41312576

**redux数据管理**

关键词：action、reducer、store

action-----行为：action是把数据从应用传到store的有效载荷，他是store数据的唯一来源,store并没有去改变state来更新界面

reducer----计算比较处理数据，改变state，更新页面: reducer就是根据action的type来处理不同的事件；

store------- store就是把action和reducer联系到一起的对象，所有的数据都是被保存在store容器中。

流程：action传入type和value值——>store dispatch一个action给reduer——>reduer的action参数中接收值 ——>js页面中可以接收

流程 ：

```jsx
//store.js文件
import { createStore } from 'redux' //1.引入createStore方法
import reducer from './reducer'     //2.引入reducer数据库
const store = createStore(reducer) //3.创建数据存储仓库

export default store
```



```jsx
//todo.js  页面渲染的文件
import { Input } from 'antd'
import React, { Component } from 'react’;
Import { CHANGE_INPUT } from ‘./store/reducer'
import store from './store’   //引入store文件，这样才可以使用store里面的方法
Const ToDo = props => {
    store.subscribe(this.storeChange) //订阅store的改变
    console.log(props.content,’— content -‘)
    Const changeInputEvent = ({target})=>{
        Const action = {
            Type:CHANGE_INPUT,
            Val:target.value
        }
    }
    
    return <Input onChange = {changeInputEvent} />
}
export default ToDo

```

```javascript
//reducer.js
Const CHANGE_INPUT = ‘CHANGE_INPUT'
Const defaultState = {
    inputVal : ''
}
Const content = (state=defaultState,action)=>{
    Const { type,val } = action 
    If(type=== CHANGE_INPUT){
        Return val
    }
    Return state
}

```



踩坑点：

1.store必须是唯一的，多个store是坚决不允许，只能有一个store空间

2.只有store能改变自己的内容，Reducer不能改变

3.Reducer必须是纯函数；不能存在数据处理，reducer只做存储数据用

4.**Redux是异步的，是异步的！！！！！**