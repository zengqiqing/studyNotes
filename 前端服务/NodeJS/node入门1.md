### nodeJS是什么

1.node.js是构建在谷歌V8引擎上的，基于谷歌v8引擎构建的js运行环境，谷歌浏览器又称被为称为宿主环境。

2.nodejs使用了一个事件驱动，非阻塞I/O模型

#### node.js特性是啥，他和我们浏览器端的宿主环境到底有什么区别？nodeJS可以干嘛？

1.chrome浏览器用的同样是js引擎和模型。 理论上来说在nodejs里写js代码和在chrome里面写js几乎是没有不一样的。

2.那么他们两者不一样的地方在哪里呢？

- nodejs没有浏览器API，即document,window，浏览器文档流等等
- nodejs加了许多nodejs专属的api，例如文件系统，进程等等 ，而且他没有浏览器的安全级别的限制（浏览器有个安全沙箱，就是同源策略 ，浏览器不能操作系统api）
- 对于开发者来说，nodejs:
  - 你再chorome里面写的js是控制浏览器，而nodejs让你用类似的方式，控制整个计算机

 #### npm

执行npm init生成package.json文件，声明该文件为npm包

### Nodejs 非阻塞I/O

- I/O 即Input/Output,一个系统的输入和输出
- 阻塞I/O和非阻塞I/O的区别在于系统接收输入再到输出期间，能不能接收其他输入。（能不能一心二用的意思）
- 理解非阻塞I/O的要点：
  - 确定一个进行Input/Output的系统（确定系统边界，限定清楚角色，可以是你做input了，但output交给别人做）
  - 思考在I/O过程中，你能不能进行其他的I/O（你input了，在等待output的过程中去做其他的input,之后再回来output）



### 异步编程

nodejs回调函数格式规范：第一个接收到的回调函数是error函数，后面的才是成功或者是其他信息的结果函数

#### eventloop

每个事件按循环都是个全新的调用栈。

世界上最遥远的距离就是 eventloop所造成。



#### promise

执行then和catch会返回一个新的promise,该promise最终状态根据then和catch的回调函数的执行结果决定

- 如果回调函数（就是说即使你再then后面，虽然按平常操作是接收正常的成功回调，但如果你还是使用throw的话，那其实最终也会是执行reject）最终是throw，该promise是rejected状态。
- 如果回调函数最终是return，该promise是resolved状态。
- 如果回调函数最终return了一个promise，该promise会和回调函数return的promise状态保持一致。

