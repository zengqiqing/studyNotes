##### 为什么this.setState是异步的？

```
setState并不总是立即就更新组件的，setState是通过一个队列机制实现state的更新并且更新的过程是一个浅合并，setState将要需要更新合并的值会被压入队列中，积攒完更新的内容后再进行批量更新处理（这样做的目的是为了把Virtual DOM和DOM树操作降到最小，用于提高性能），而不是立即执行this.setState来进行修改，从而出现异步的场景。
```

##### setState就一定是异步了吗？



##### 为何react不同步更新this.state呢？

官网给出的原因：

	- 这样会破坏掉props和state之间的一致性，造成一些难以debug的问题。
	- 这样会让一些我们正在实现的新功能变得无法实现