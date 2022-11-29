## Redux 的api使用：

**combineReducers：**

用于Reducer的拆分，只要定义各个子 Reducer 函数后使用combineReducers即可将它们合成一个大的 Reducer啦~

语法：

```jsx
//以下代码通过combineReducers方法将三个子 Reducer 合并成一个大的函数
import { combineReducers } from 'redux';
const chatReducer = combineReducers({
  chatLog,
  statusMessage,
  userName
})
export default todoApp;

```



1.在createStore上使用，配合redux的applyMiddleware中间件来使用

2.放在createStore函数的第二个参数中

```jsx
//语法

import { createStore, applyMiddleware } from 'redux' //引入createStore方法

import reducer from './reducer' //引入reducer数据库

import thunk from 'redux-thunk' //引入thunk库

const middleware =applyMiddleware(thunk)

const store = createStore(reducer, middleware) //创建数据存储仓库

export default store
```



3.可以到你的action中写接口啦~~

```jsx
//语法
import { GROUP_LIST } from './type';
import axios from 'axios';
const TOKEN =
  'eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJTSUJVIiwiZXhwIjoxNTg0NjMyMDQ0LCJhdWQiOiJXTEpTIiwianRpIjoiNDIzMSIsInN1YmplY3QiOnsidXNlcklkIjo0MjMxLCJ1c2VybmFtZSI6IuWkp-aZtCIsInJvbGVJZExpc3QiOlsxLDEwMDMxXSwidG9rZW4iOm51bGx9fQ.ANe-nu1bYew6jKSE2l1FsPSzTUzuWgVMHzVppHeWQ-w';
export const groupAction = data => {
    Type: GROUP_LIST, Data;
};
export const getGroup_fetch = () => {
    return dispatch => {
        axios
            .get('https://soul-uat.weilaijishi.com/admin-returns/customer/group/grouplist', {
                headers: { token: TOKEN }
            })
            .then(res => {
                const { data } = res;
                const { errorCode, success, result } = data;
                if (errorCode === 0 && success && result && result.length) {
                    const action = groupAction(result);
                    dispatch(action);
                }
            });
    };
};

```



4.todo.js页面调用存在于action的内容

```jsx
import React, { Component } from 'react';
import 'antd/dist/antd.css';
import store from './store’
import { getGroup_fetch } from './store/actionCreators’
const Todo = props => {
    constructor(props) {
        super(props)
        store.subscribe(this.storeChange) //订阅store的改变
    }
    componentDidMount(){
        const action = getGroup_fetch()
        store.dispatch(action)
    }
    return (
        …...
    )
}
export default Todo

```

