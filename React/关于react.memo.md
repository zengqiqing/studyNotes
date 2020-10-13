### React.memo的使用

## 一、React.memo()



### 1、React.memo 的使用方式

React.memo() 文档地址：

- https://reactjs.org/docs/react-api.html#reactmemo

在 class component 时代，为了性能优化我们经常会选择使用 `PureComponent`,每次对 props 进行一次浅比较，当然，除了 `PureComponent` 外，我们还可以在 shouldComponentUpdate 中进行更深层次的控制。

在 Function Component 的使用中， React 贴心的提供了 `React.memo` 这个 HOC（高阶组件），与 PureComponent 很相似，但是是专门给 Function Component 提供的，对 Class Component 并不适用。

相比于 PureComponent ，React.memo() 可以支持指定一个参数，可以相当于 shouldComponentUpdate 的作用，因此 React.memo() 相对于 PureComponent 来说，用法更加方便。

（当然，如果自己封装一个 HOC，并且内部实现 PureComponent + shouldComponentUpdate 的结合使用，肯定也是 OK 的，在以往项目中，这样使用的方式还挺多）

首先看下 React.memo() 的使用方式：

```javascript
function MyComponent(props) {
  /* render using props */
}
function areEqual(prevProps, nextProps) {
  /*
  return true if passing nextProps to render would return
  the same result as passing prevProps to render,
  otherwise return false
  */
}
export default React.memo(MyComponent, areEqual);
```

使用方式很简单，在 Function Component 之外，在声明一个 areEqual 方法来判断两次 props 有什么不同，如果第二个参数不传递，则默认只会进行 props 的浅比较。

最终 export 的组件，就是 React.memo() 包装之后的组件



### 2、React.memo 在性能优化上的实践

理解下面三个组件代码即可理解 React.memo 的使用：

- index.js：父组件
- Child.js：子组件
- ChildMemo.js：使用 React.memo 包装过的子组件



#### 1、index.js

父组件进行的逻辑很简单，就是引入两个子组件，并且将三个 state 通过 props 的方式传递给子组件

父组件本身进行的逻辑会进行三个 state 的变化

理论上，父组件每次变化一个 state 都通过 props 传递给了子组件，那子组件就会重新执行渲染。（无论子组件有没有真正用到这个 props）

```javascript
// 父组件
import React, { useState, } from 'react';
import Child from './Child';
import ChildMemo from './Child-memo';

export default (props = {}) => {
    const [step, setStep] = useState(0);
    const [count, setCount] = useState(0);
    const [number, setNumber] = useState(0);

    const handleSetStep = () => {
        setStep(step + 1);
    }

    const handleSetCount = () => {
        setCount(count + 1);
    }

    const handleCalNumber = () => {
        setNumber(count + step);
    }


    return (
        <div>
            <button onClick={handleSetStep}>step is : {step} </button>
            <button onClick={handleSetCount}>count is : {count} </button>
            <button onClick={handleCalNumber}>numberis : {number} </button>
            <hr />
            <Child step={step} count={count} number={number} /> <hr />
            <ChildMemo step={step} count={count} number={number} />
        </div>
    );
}
```



#### 2、Child.js

这个子组件本身没有任何逻辑，也没有任何包装，就是渲染了父组件传递过来的 `props.number`。

需要注意的是，子组件中并没有使用到 `props.step` 和 `props.count`，但是一旦 props.step 发生了变化就会触发重新渲染

```javascript
import React from 'react';

export default (props = {}) => {
    console.log(`--- re-render ---`);
    return (
        <div>
            {/* <p>step is : {props.step}</p> */}
            {/* <p>count is : {props.count}</p> */}
            <p>number is : {props.number}</p>
        </div>
    );
};
```



#### 3、ChildMemo.js

这个子组件使用了 React.memo 进行了包装，并且通过 `isEqual` 方法判断只有当两次 props 的 `number` 的时候才会重新触发渲染，否则 `console.log` 也不会执行。

```javascript
import React, { memo, } from 'react';

const isEqual = (prevProps, nextProps) => {
    if (prevProps.number !== nextProps.number) {
        return false;
    }
    return true;
}

export default memo((props = {}) => {
    console.log(`--- memo re-render ---`);
    return (
        <div>
            {/* <p>step is : {props.step}</p> */}
            {/* <p>count is : {props.count}</p> */}
            <p>number is : {props.number}</p>
        </div>
    );
}, isEqual);
```

<font color=red>或者不传入一些子组件用不到的参数再结合React.memo来使用的那，那也不会触发重新渲染</font>请注意下面代码中的ChildMemo的传值

```javascript
// 父组件

import React, { useState, } from 'react';
import Child from './Child';
import ChildMemo from './ChildMemo';

export default (props = {}) => {
    const [count, setCount] = useState(0);
    const [step, setStep] = useState(0);
    const [number, setNumber] = useState(0);
    const [obj, setObj] = useState({id:1});

    const handleSetStep = () => {
        setStep(step + 1);
    }

    const handleSetCount = () => {
        setCount(count + 1);
    }

    const handleCalNumber = () => {
        setNumber(count + step);
    }

    return (
        <div>
            <button onClick={handleSetStep}>step is : {step} </button>
            <button onClick={handleSetCount}>count is : {count} </button>
            <button onClick={handleCalNumber}>numberis : {number} </button>
            <hr />
            <Child step={step} count={count} number={number} /> <hr />
            <ChildMemo number={number} />
        </div>
    );
}
```



#### 4、效果对比：

![GIF.gif](http://www.ptbird.cn/usr/uploads/2019/12/2715981595.gif)

通过上图可以发现，在点击 step 和 count 的时候，props.step 和 props.count 都发生了变化，因此 `Child.js` 这个子组件每次都在重新执行渲染（`----re-render----`），即使没有用到这两个 props。

而这种情况下，`ChildMemo.js` 则不会重新进行 re-render。

只有当 props.number 发生变化的时候，`ChildMemo.js` 和 `Child.js` 表现是一致的。

从上面可以看出，**React.memo() 的第二个方法在某种特定需求下，是必须存在的。** 因为在实验的场景中，我们能够看得出来，即使我使用 `React.memo` 包装了 Child.js，也会一直触发重新渲染，因为 props 浅比较肯定是发生了变化。



## 二、使用 useMemo() 进行细粒度性能优化

上面 React.memo() 的使用我们可以发现，最终都是在最外层包装了整个组件，并且需要手动写一个方法比较那些具体的 props 不相同才进行 re-render。

而在某些场景下，我们只是希望 component 的部分不要进行 re-render，而不是整个 component 不要 re-render，也就是要实现 `局部 Pure` 功能。

`useMemo()` 基本用法如下：

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

useMemo() 返回的是一个 memoized 值，只有当依赖项（比如上面的 a,b 发生变化的时候，才会重新计算这个 memoized 值）

memoized 值不变的情况下，不会重新触发渲染逻辑。

说起渲染逻辑，需要记住的是 useMemo() 是在 render 期间执行的，所以不能进行一些额外的副操作，比如网络请求等。

如果没有提供依赖数组（上面的 [a,b]）则每次都会重新计算 memoized 值，也就会 re-redner

上面的代码中新增一个 `Child-useMemo.js` 子组件如下：

```javascript
import React, { useMemo } from 'react';

export default (props = {}) => {
    console.log(`--- component re-render ---`);
    return useMemo(() => {
        console.log(`--- useMemo re-render ---`);
        return <div>
            {/* <p>step is : {props.step}</p> */}
            {/* <p>count is : {props.count}</p> */}
            <p>number is : {props.number}</p>
        </div>
    }, [props.number]);
}
```

与上面唯一的区别是使用的 `useMemo()` 包装的是 return 部分渲染的逻辑，并且声明依赖了 props.number，其他的并未发生变化。

效果对比：

![GIF2.gif](http://www.ptbird.cn/usr/uploads/2019/12/1933404739.gif)

上面图中我们可以发现，父组件每次更新 step/count 都会触发 useMemo 封装的子组件的 re-render，但是 number 没有变化，说明并没有重新触发 HTML 部分 re-render

只有当依赖的 props.number 发生变化的时候，才会重新触发 useMemo() 包装的里面的 re-render



ps:看回以前同事写的一些代码，

```javascript
import React from "react";

export default React.memo((props) => {
  
  const columns0 = [{title:'good',id:0}] //如果其余地方触发了setState，那么columns0会重新被定义，消耗性能
  
	const columns1 = React.useMemo(()=>{
		return [{title:'hello',id:1,name:props.menuInfos.name}]
	},[props.menuInfos]) //如果props.menuInfos发生改变了，那么columns1才会被重新定义，即使发生setState也不会重新渲染

	const columns2  = React.useMemo(()=>{
    retrun [title:'hi',id:2]
  },[]) //仅初始化一遍，之后其余地方触发了setState也会不再次被触发
})

```

