一：先来看看单纯使用组件，不添加任何优化手段。Acomp有接受父组件home传入参数val，每当val发生变化时，Acomp都会重新渲染这是没问题的。但看Bcomp没有接受任何参数，但由于Acomp引发的重复渲染牵连到父函数home组件

```javascript
import  React, { memo, useState, useEffect } from  'react'

const  Acomp = (props) => {
console.log('A1')
useEffect(() => {
	console.log('A2')
})
return  <div>A</div>
}

const  Bcomp = (props) => {
console.log('B1')
useEffect(() => {
	console.log('B2')
})
return  <div>B</div>
}

const  Home = (props) => {
const [a, setA] = useState(0)
useEffect(() => {
console.log('start')
setA(1)
}, [])
return (
	<div>
  	<Acomp  val={a}  />
		<Bcomp />
  </div>
)}
export default Home;
打印结果：
A1
B2
start
A2
B2

```

##### 场景提问1：为何父组件渲染数据更新了，子组件接收的参数并没有发生改变，为何也会重新渲染呢？

```
只要父组件render了，那么默认情况下就会触发子组件的render过程，若子组件中包含孙组件，那么孙组件又会被触发重新render，当render过程到了叶子节点，diff过程便会开始，此时diff算法会决定是否切实更新DOM元素。

由于React不能检测到是否给子组件传属性，所以他必须要有这个重新渲染的过程（术语叫做reconciliation），因为reconciliation过程是执行的js，而重新渲染的性能开销主要是更新DOM导致的，最后diff算法会介入是否需要更新真正的dom的，js的脏歘承诺速度很快，所以即使父组件的render会触发所有后代组件的render过程，但这个效率并没有太大的影响

当然从道理上讲，既然我没有给子组件传递属性，或我的程序能判断出传递的属性是否有发生变化，那么无需进行子组件的重新渲染过程，但react无法提供自动检测这一点，所以react提供了shouldComponmentUpdate回调函数，让我们根据实际情况去决定是否需要重新render组件。如果某个组件的shouldComponentUpdate总是返回false, 那么当它的父组件render了，会触发该组件的render过程，但是进行到shouldComponentUpdate判断时会被阻止掉，从而就不调用它的render方法了，它自己下面的组件的render过程压根也不会触发了
```

##### 场景提问2：同在一个父组件内的子组件a和子组件b，为何子组件a发生重新渲染时，子组件也会被重新渲染了呢？