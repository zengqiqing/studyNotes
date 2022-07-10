

### 中间件

- 中间件（middleware）特指业务流程中的处理环节

- express中间件的调用流程：

当一个请求到达express服务器时，可以连续调用多个中间件，从而对这次请求进行预处理。

```
客户端发起请求-> 中间件1-> 中间件2-> 中间件n->处理完毕，响应这次请求 ->响应客户端
```

express的中间件，本质上是一个处理函数，express中间件格式如下：

```
const express = require('express')
const app = express()

app.get('/',(req,res,next)=>{
	next()
})
app.listn(3000)
```

中间件函数的形参列表中，必须包含next参数，而普通路由处理函数中则只包含req和res。也就是说，如果你有看到参数中含有next,那这个函数就是中间件函数。

next函数是实现多个中间件连续调用的关键，他表示将流转关系转交给下一个中间件或路由。我的理解是next是指针一样，当执行完一个中间件后，通过调用next告诉下一个中间件做进一步处理了！！

```javascript
const express = require('express')
const app = express()

const mw = (req,res,next)=>{
    console.log('这是最简单的中间件函数')
    next()//将关系流转，转交给下一个中间件或路由
}

app.listen(80,()=>{
    console.log('http://127.0.0.1')
})
```



### 全局生效的中间件

客户端发起的任何请求，到达服务器之后，都会触发的中间件，叫做全局生效的中间件。通过调用app.use(中间件函数)，即可定义一个全局生效的中间件，示例代码如下：

```javascript
const express = require('express')
const app = express()

const mw = (req,res,next)=>{
    console.log('这是最简单的中间件函数')
    next()//将关系流转，转交给下一个中间件或路由
}

// 将mw注册为全局生效的中间件
app.use(mw)

app.get('/',(req,res)=>{
    console.log('调用了home')
    res.send('home page')
})

app.listen(80,()=>{
    console.log('http://127.0.0.1')
})

//注意，app.use代码如果放在请求代码后面，那么app.use则不被调用，所以要走中间件，请求代码必须放在app.use的代码后面
```

#### 中间件的作用

多个中间件之间，共享同一份req和res，基于这样的特性，我们可以在上游的中间件中，统一为req或者res对象添加自定义的属性或方法，共下有的中间件或路由进行使用。

```javascript
const express = require('express')
const app = express()



// 将mw注册为全局生效的中间件
app.use((req,res,next)=>{
    // 获取到请求到达服务器的时间
    const time = Date.now()

    // 为req对象，挂载自定义属性，从而把事件共享给后面的所有路由
    req.startTime = time

    next()//将关系流转，转交给下一个中间件或路由
})
app.get('/',(req,res)=>{
    res.send('home page'+req.startTime)
})

app.get('/user',(req,res)=>{
    res.send('user page'+req.startTime)
})

app.listen(80,()=>{
    console.log('http://127.0.0.1')
})
```

### 定义多个全局中间件

可以使用app.use()连续定义多个全局中间件。客户端请求到达服务器后，会按照中间件定义的先后顺序一次进行调试。

```javascript
const express = require('express');

const app = express()

app.use((req,res,next)=>{
    console.log('调用了第一个中间件')
    next()
})

app.use((req,res,next)=>{
    console.log('调用了第二个中间件')
    next()
})

app.get('/',(req,res)=>{
    console.log('调用了home')
    res.send('home page')
})
app.listen(80,()=>{
    console.log('http://127.0.0.1')
})
```

### 局部生效的中间件

不适用app.use（）定义的中间件，叫做局部生效的中间件。

```javascript
// 局部中间件

const express = require("express")

const app = express()

const mw1 = function(req,res,next){
    console.log('这是中间件函数啊！')
    next()
}
// mw1中间件只在当前路由中生效，这种用法属于局部生效的中间件
app.get('/',mw1,(req,res)=>{
    res.send('home page')
})

// 这里不经过局部生效的中间件哦！
app.get('/user',(req,res)=>{
    console.log('调用了user')
    res.send('user page')
})

app.listen(80,()=>{
    console.log('http://127.0.0.1')
})
```

### 定义多个局部中间件

可以在路由中，通过如下两种等价方式，使用多个局部中间件

```javascript
app.get('/',mw1,mw2,(req,res)=>{
    res.send('home page')
})

app.get('/',[mw1,mw2],(req,res)=>{
    res.send('home page')
})
```

### 了解中间件的5个使用注意事项

1.一定要在路由之前注册中间件，否则中间件不被调用

2.客户端发送请求，可以连续调用多个中间件进行处理

3.执行完中间件的业务代码后，不要忘记调用next()函数

4.为了防止代码逻辑混乱，调用next()函数后不要写额外的代码了，因为执行了next代表执行完该中间件，下一个中间件准备执行了

5.连续调用多个中间件时，多个中间件之间，共享req和res对象

#### 中间件的分类

express官方常见的中间件用法，分成5大类：

1.应用级别的中间件：通过app.use或app.get或app.post，绑定到app实例上的中间件，叫做应用级别的中间件

2.路由级别的中间件：绑定到express.Router实例上的中间件，叫做路由级别的中间件。他的用法和应用级别中间件没有去呗，只不过应用级别中间件是绑定到app上，路由级别中间件绑定到router实例上

3.错误级别的中间件：专门用来捕获整个项目中发生的异常错误，从而防止项目崩溃问题。错误级别的中间件处理函数中，必须要要有4个形参，形参顺序从前到后，分别是（err,req,res,next ）

```
app.get('/errortext',mw1,(req,res)=>{
    throw new Error('服务器内部发生了错误')
    res.send('error page')
})
```

注意：错误级别的中间件，必须放在路由之后,其他的中间件，必须在路由之前进行配置，下面是错误的例子，errortext的路由在异常中间件之后，当调用时，直接报错，没有走到异常中间件中

```
// 捕获异常中间件
app.use((err,req,res,next)=>{
    console.log('发生错误！'+err.message)
    res.send('error msg '+err.message)
})

app.get('/errortext',mw1,(req,res)=>{
    throw new Error('服务器内部发生了错误')
    res.send('error page')
})
```

4.express内置的中间件：express内置了3个常用的中间件，极大提高了express项目的开发效率和体验：

① express.static快速托管静态资源的内置中间件，例如html文件，图片，css样式等（无兼容性）

②express.json解释json格式的请求体数据（有兼容性，仅在4.16.0+版本中可用）

```
//配置解析application/json数据的内置中间件
app.use(express.json())
//配置解析application/x-www-form-urlencoded格式数据的内置中间件
app.use(express.urlencoded({extended:false}))
```



③express.urlencoded解析URL-encoded格式的请求体数据（有兼容性，仅在4.16.0+版本中可用）

5.第三方的中间件:第三方开发出来的中间件

①.npm install 第三方包

②.使用require导入中间件

③.调用app.user注册并使用中间件



### 接口的跨域问题

CORS响应头部中可以携带一个Access-Control-Allow-Origin字段，语法如下：

```
Access-Control-Allow-Origin:<origin>|*
```

其中，origin参数的值指定了允许访问该资源的外域URL

例如下面的字段值将只允许来自http://itcast.cn的请求：

```
res.setHeader('Access-Control-Allow-Origin','http://itcast.cn')
```

如果指定了Access-Control-Allow-Origin字段值为通配符*，表示允许来自任何域的请求

```
res.setHeader('Access-Control-Allow-Origin','*')
```


默认情况下，CORS仅支持客户端向服务器发送如下的9个请求头：
Accept. Accept-Language. Content-Languageₓ DPR、Downlink. Save-Dataₓ Viewport-Width. Width、
Content-Type （值仅限于 text/plain, multipart/form-data. application/x-www-form-urlencoded 三者之一）
如果客户端向服务器发送了额外的请求头信息，则需要在服务器端，通过Access-Control-Allow-Headers对额外 的请求头进行声明，否则这次请求会失败！

#### 预检请求

  只要符合以下任何一个条件的请求，都需要进行预检请求:
① 请求方式为GET、POST、HEAD之外的请求Method类型
②请求头中包含自定义头部字段
③ 向服务器发送了 application/json格式的数据
在浏览器与服务器嗯通信之前，浏览器会先发送OPTION请求进行预检，以获知服务器是否允许该实际请求，所以这一 次的OPTION请求标为"预检请求"。服务器成功响应预检请求后，才会发送真正的请求，并且携带真实数据。

简单请求的特点：客户端与服务器之间只会发生一次请求

预检请求的特点：客户端与服务器之间发生两次请求，option预检请求成功后，才会发起真正的请求

### JSONP接口

概念：浏览器端通过scrpit标签中的src属性，请求服务器上的数据，同时，服务器返回一个函数的调用，这种请求数据的方式叫做jsonp

jsonp不属于真正的ajax请求，因为他没有使用XMLHttpRequest这个对象

json仅支持get请求不支持get以外的请求