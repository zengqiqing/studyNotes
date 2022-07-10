初识http：

http模块是nodejs官方提供的，用来创建web服务器的模块，通过http模块提供的http.createServer()方法，使用require导入使用，就能方便地将一台普通的电脑编程一台web服务器，从而对外提供web资源服务。

域名和域名服务器：

ip地址和域名是一一对应的关系，这份对应关系存放在一种叫做域名服务器（DNS Domain name sever）的电脑中使用者只需通过好记的域名访问对应的服务器即可。对应的转换工作由域名服务器实现，因此，域名服务器就是提供ip地址和域名之间的转换服务的服务器。

端口号：

在实际应用中，url中的80端口是可以被省略的，注只有80端口可以省略非80端口不能省略，例如：http://127.0.0.1:80 =转换成> http://127.0.0.1



### 创建最基本的web服务器

```javascript
// 1.通过require导入http模块
const http = require('http');

// 2.创建web服务器实例
const server = http.createServer();

// 3.为服务器实例绑定request事件，监听客户端的请求
server.on('request',(req,res)=>{
    console.log('someone visit our web server.');
})

// 4.启动服务器
server.listen(8080,()=>{
    console.log('当前正在监听http://127.0.0.1:8080');
})
```

### req请求对象

只要服务器接收到客户端的请求，就会调用通过server.on为服务器绑定的request事件处理函数。如果想在事件处理函数中，访问与**客户端**相关的数据或属性，可以使用如下方法：

```javascript
const http = require('http');

const server = http.createServer();

server.on('request',(req,res)=>{
    // req.url是客户端请求的url地址
   const url = req.url
    // req.method是客户端请求的method方法,浏览器默认请求get请求
    const method = req.method

    const str = `your request url is ${url},and request method is ${method}!`
    console.log(str);
})
// 启动服务器
server.listen(8080,()=>{
    console.log('当前正在监听http://127.0.0.1');
})
```

如何请求post请求？

到postman中，发起一个post请求，请求的地址就是上面起的ip地址`http://127.0.0.1:8080/about.html`

总结：

req是请求对象，它包含与客户端相关的数据和属性

req.url可以获取到访问域名时，客户端发起请求的地址

req.method可以获取到访问域名时，客户端发起请求的方法

### res响应对象

可以使用res.end()方法向客户端发送指定内容，并结束此次请求的过程。

```javascript
const http = require('http');

const server = http.createServer();

server.on('request',(req,res)=>{
    // req.url是客户端请求的url地址
   const url = req.url
    // req.method是客户端请求的method方法,浏览器默认请求get请求
    const method = req.method
    const str = `your request url is ${url},and request method is ${method}!`
    console.log(str);

    // 调用res.end方法向客户端响应内容
    res.end(str)

})

// 启动服务器
server.listen(8080,()=>{
    console.log('当前正在监听http://127.0.0.1:8080');
})
```



### 解决中文乱码的问题

当调用res.end()方法，向客户端发送中文内容时，会出现乱码问题，此时需要我们手动设置响应头的编码格式：

```
res.end('请搜索"飞书",搜索你想要的konw')
```

![image-20220329202414788](/Users/cole/Library/Application%20Support/typora-user-images/image-20220329202414788.png)

使用res.setheader()方法解决

```javascript
const http = require('http');

const server = http.createServer();

server.on('request',(req,res)=>{
    const str = `您请求的url地址是${req.url}，请求的method类型是${req.method}！`
    res.setHeader('Content-Type','text/html;charset=utf-8');
    res.end(str)
})

// 启动服务器
server.listen(8080,()=>{
    console.log('当前正在监听http://127.0.0.1:8080');
})
```

//不要搞错啦！！！req可以获取url和请求方式，res是用来设置请求头和启动服务哒！