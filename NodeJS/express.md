#### 什么是express

官方给出的概念：express是基于nodejs平台，快速，开放，极简的web开发框架。

通俗的理解：express的作用和nodejs内置的http模块类似，是专门用来创建web服务器的。

express的本质：就是一个npm上的第三方包，提供了快速创建web服务器的便捷方法。

#### 如何理解express？

http内置模块用起来复杂，开发效率低而express是基于nodejs的内置http模块进一步封装出来 ，能够极大提高开发效率。通俗点理解express就像是框架，类比于浏览器中的web API和jq的关系，后者是基于前者进一步封装出来的。 



#### express的demo

```javascript
const express = require("express");

const app = express();

// 监听get & post请求，并向客户端响应内容
app.get("/user", (req, res) => {
  // express提供的响应方法：send 他既能响应对象也能响应文本
  res.send({ name: "lily", age: 20, gender: "boy" });
});
app.post("/user", (req, res) => {
  res.send("请求成功！");
});

app.get("/", (req, res) => {
  // 通过req.query可以获取到客户端发送过来的查询参数，注意，默认情况下，req.query是一个空对象
  // console.log(req.query)//{ name: 'coco', age: '23' }
  res.send(req.query);
});

// url地址中，可以通过：参数名的形式，匹配动态参数值，url的调用：http://127.0.0.1:90/user/3
// 冒号后面的字符串是可以任意定义的，只要合理合法就可以
app.get("/user/:idkkk", (req, res) => {
  // 通过req.params动态获取到参数值
//   console.log(req.params); //{"idkkk": "3"}
  res.send(req.params);
});

// 可以传入多个动态参数
app.get("/user/:idkkk/:names", (req, res) => {
    // 通过req.params动态获取到参数值
    console.log(req.params); //{ idkkk: '3', names: 'coco' }
    res.send(req.params);
  });

app.listen(90, () => {
  console.log("express server running at http://127.0.0.1");
});

```

#### 托管静态资源

`express.static()`

express提供了一个非常好用的函数，叫做express.static()，通过它，我们可以非常方便地创建一个静态资源服务器。

例如，通过如下代码就可以将public目录下的图片，css,js文件对外开放访问了。

```
app.use(express.static(public))

//现在你可以访问public目录中的所有文件啦~~
http://localhost:3000/image/bg.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/login.js

//为什么http://localhost:3000/js/login.js中没有包含public啊？他不是应该为http://localhost:3000/public/js/login.js吗？
//因为：注意express在指定目录中查找文件，并对外提供资源的访问路径，因此，存放静态文件的目录名不会出现在url中
```

#### express路由

路由就是映射关系，路由匹配的注意点：1.按照定义的先后顺序进行匹配；2.请求methods和请求的url同时匹配成功才会调用对应的处理函数。

为了方便对路由进行模块化管理，express不建议将路由直接挂在到app上，而是推荐将路由抽离为单独模块。

