### 关于react原理 ---- day 1

#### 什么是虚拟dom

就是用js对象描述真正的dom树，当状态变更时，重新渲染这个js的对象结构，这个js的对象就被称为虚拟dom，避免每次的更新都需要重新更新所有的dom节点



为什么要用虚拟dom?

dom的操作是很慢的并且轻微的操作都可能导致页面重排，所以直接操作真实的dom是非常消耗性能，相对于dom对象，js对象处理起来更快且更简单，通过diff算法对比新旧dom之间的差异，可以批量的，最小化地执行dom操作，从而提高性能。并且使用js对象解决了跨平台的问题，各端支持js对象，避免了dom的api的兼容性问题，提升了开发基础成本



jsx实际上转译过来后他们就变成React.createElement（。。。）的形式，该函数将生成vdom来描述真实的dom，但这种方式已经过时了，现在17版的jsx有重构？？？查一下文档



查看源码：

主要的东西在package文件，但先从index.js入口文件去看，看src的react.js文件，里面放了很多react的api的合集

1.看component 

看component 和purComponent，purComponent有使用到componentUpdate的方法

老师讲component的时候提到一个forceupdate，没了解过，要去补补



2.老师讲render函数

![image-20210307204348157](/Users/cole/Library/Application Support/typora-user-images/image-20210307204348157.png)



3.createElement其实是个js对象，接收传入的每个参数

看视频的时候有注意到$$typeof，他是和xss攻击有关系

https://zhuanlan.zhihu.com/p/53163790

cloneElement:在原先的基础进行克隆



