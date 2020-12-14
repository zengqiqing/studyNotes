## 关于 promise,async/await 的笔记

- ### <font color='red'>promise</font>

  promise/A+规范文章:https://promisesaplus.com/

  - ### promise 是什么，能解决什么问题?

    - Promise 是解决异步编程的一种解决方案.他的出现是为了解决传统回调函数带来的回调地狱问题.

  - ### promise 语法解析:

    - 从语法来讲,promise 是一个对象,new Promise时，需要传递一个executor执行器，执行器立即执行。

    - 执行器中传递两个参数： `resolve(成功)`  `reject(失败)` 

    - promise只能从 pending 到 rejected,或者从pending 到 fulfilled （成功）状态。promise一旦执行不能中断，确认并返回状态后，promise则执行完成。

    - promise拥有.then 和.catch链式调用方法,then能接收 promise(onFulfilled) 成功的回调,catch接收失败的回调（onrejected）.若一个 promise 里面都有写.then 和.catch的捕捉,那么只要 promise 返回状态,那么这个promise 的状态就属于执行完毕,返回fulfilled.若没有写.catch 那么返回rejected告知开发者promise 出错但未进行捕抓.

    - 如果promise的状态是pending，需要将onFulfilled和onRejected函数存放起来，等待状态确定后，再依次将对应的函数执行(发布订阅).

    - 如果 then 返回的是一个promise,那么需要等这个promise，那么会等这个promise执行完，promise如果成功， 就走下一个then的成功，如果失败，就走下一个then的失败

      

  - 语法：

    ```javascript
    promiseClick = () =>{
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
                var num = Math.ceil(Math.random() * 20); //生成1-10的随机数
                if (num <= 10) {
                    resolve(num);
                }
                else {
                    reject(num);
                }
            },500)
        })
    }
    
    promiseClick()
    .then(res=>{
        console.log(res,'数字小于10,执行成功回调')
    })
    .catch(err=>{
        console.log(err,'数字大于10,执行失败回调')
    })
    
    /**
    	*回调成功时的打印: Promise{<fulfilled>:3'数字小于10,执行成功回调'}
    	*回调成功时的打印: Promise{<fulfilled>:12'数字小于10,执行失败回调'}
    	*当前函数执行reject，在then 里面是不能接收到err,而是在 catch 里面接收.若开发者没有使用.catch 捕抓错误时,Promise将返回<rejected>
    **/
    ```

  - ### <font color='red'>promise 周边</font>

    #### Promise.finally()

    不管promise最后的状态，在执行完then或catch指定的回调函数以后，都会执行finally方法指定的回调函数。

    ```javascript
    //语法
    promise
    .then(result => {···})
    .catch(error => {···})
    .finally(() => {···});
    ```

    #### promise.all(同生共死)

    实战场景:Promse.all在处理多个异步处理时非常有用，比如说一个页面上需要等两个或多个ajax的数据回来以后才正常显示，在此之前只显示loading图标。

    - Pormise.all可以将多个 promise实例包装成一个新的 promise.all结果数据在数组中的排序和请求所需的时间无关，与请求数组事件的排列顺序有关系。就是这个语句:`Promise.all([func2(),func1()])`
    - 如果其中接口出现 reject情况,那么只会返回错误的内容信息,另一个成功接口的内容是不会 resolve 出来的.如果 2 个接口都出现 reject情况,那么会先返回语法中排第一个的接口的错误信息出来
    - 如果参数中有一个promise失败，那么Promise.all返回的promise对象失败
    - 在任何情况下，Promise.all 返回的 promise 的完成状态的结果都是一个数组

    ```javascript
    var func1 = () => {
        return new Promise ((resolve,reject)=>{
        setTimeout(()=>{
            resolve(1111)
        },500)
    })
    }
    
    var func2= () => {
          return new Promise ((resolve,reject)=>{
        setTimeout(()=>{
            resolve(2222)
        },1000)
    })
    }
    
    var func = ()=>{
        return  Promise.all([func2(),func1()]).then(res=>{
        console.log(res)
    })
    }
    
    func()
    [2222,1111]
    ```

    #### Promise.race(谁先回来返回谁)

    all是等所有的异步操作都执行完了再执行then方法，那么race方法就是相反的，谁先执行完成就先执行回调。all是回调一个固定的数组，race是谁先回调回来，就回调谁的数据类型和内容.

    如果其中一个接口出现 reject 的情况,那么还是看请求速度的,谁先回来就返回谁的result

    ```javascript
    var func1 = () => {
        return new Promise ((resolve,reject)=>{
        setTimeout(()=>{
            resolve(1111)
        },500)
    })
    }
    
    var func2= () => {
          return new Promise ((resolve,reject)=>{
        setTimeout(()=>{
            resolve(2222)
        },500)
    })
    }
    
    var func = ()=>{
        return  Promise.race([func1(),func2()]).then(res=>{
        console.log(res,'res')
    }).catch(err=>{
        console.log(err,'err')
        })
    }
    
    func()//1111
    ```

    

  - #### 延伸问题：

    - 如果我的promise一直不resolve或者不reject的话，一直处在 pedding 状态,那么会造成内存泄漏吗?

      ```text
      v8对于 Promise 的实现存在内存泄漏问题，当一个 promise 无法 resolve 也无法 reject 的时候，就会发生内存泄漏.一个很容易造成 Promise 内存泄漏的场景便是递归 Promise 或者嵌套 	Promise
      参考文章:http://www.alloyteam.com/2015/05/memory-leak-caused-by-promise/
      ```

    

    - 如果 promise.all 请求的是相同的 url,那么返回来的数据,你知道哪个 result 是哪一个 ur的吗?

      ```text
      知道的,因为 promise.all 返回体是跟请求时的数组的排列顺序有关系的,先请求谁就返回谁的数据先
      ```

    - 在 promise 里面使用 try/catch 如何?

      ```
      promise的错误需要通过回调函数来捕获,promise本身自带 executor执行器,提供了返回状态的函数了,并且其返回状态是通过executor执行器里面的方法来处理的,try/catch 是捕获不了错误的
      ```

      

    - 如何获取 setTimeout里面的计算结果?

      ```javascript
      //这个问题来记录一下,我一开始的写法:👇
      function fn() {
          var data;
          setTimeout(function () {
              data = "hello"
          }, 1000)
          return data
      }
      var val = fn()
      console.log(val)//undefined
      
      //为啥 undefined呢??因为异步啊,data都还没来得及赋值就 return 出去了;咋办?看下面👇
      
      // 方法一:通过传入回调函数
      function fn(cb) {
          var data
          setTimeout(function () {
              data = "hello"
              cb(data)
          }, 1000)
      }
      fn(res=>{
          console.log(res,'打印回调值')
      })// hello 打印回调值
      
      //方法二:通过 promise 解决
      function fn(cb) {
          return new Promise((resolve,reject)=>{
             setTimeout(function () {
                  resolve('hello')
              }, 1000) 
          })
      }
      fn().then(res=>{
          console.log(res,'打印回调值')
      })// hello 打印回调值
      ```

      

- ### Async/await

  

  	- async函数的实现原理,是将 Generator函数和自执行器,包装在一个函数里.async/Await的作用就是把异步变成同步,他是异步编程的终极方案

  - Async 函数返回一个 promise 对象,可以使用 then 方法添加回调函数.当函数执行的时候,一旦遇到 await 就先返回.等到异步操作完成,再接着执行函数体内后面的语句.

  ```javascript
  //基础语法
  function timeout(ms){
      return new Promise((resolve)=>{
      setTimeout(resolve,ms)
  })
  }
  ​
  async function asyncPrint(value,ms){
      await timeout(ms)
      console.log(value)
  }
  asyncPrint('hello',500)
  
  ```

- async 函数返回的 promise 对象,必须等到内部所有的 await 命令后面的 promise 对象执行完才会发生状态改变,除非遇到 return 语句或者抛出错误.也就是说只有 async 函数内部的异步操作执行完,才会执行 then 方法指定的回调函数

- 正常情况下,await命令后面是一个 promise 对象,返回该对象的结果,如果不是 promise 对象就直接返回对应的值

  ```javascript
  async function f() {
    // 等同于
    // return 123;
    return await 123;
  }
  
  f().then(v => console.log(v))
  ```

- 任何一个 await 语句后面的 promise 对象变为 reject 状态,那么整个 async函数都会中断执行

  ```javascript
  async function f() {
    await Promise.reject('出错了');
    await Promise.resolve('hello world'); // 不会执行
  }
  ```

- 如果希望即使前一个异步操作失败,也不要中断后面的异步操作,此时可以将第一个 await 放在 try/catch 结构里面

  ```javascript
  async function f(){
      try{await Promise.reject('出错了')}
      catch(e){
          console.log(e,'catch 里面的打印')
      }
      return await Promise.resolve('hello')
  }
  f().then(v=>{console.log(v)})
  .catch(err=>{console.log(err)})
  //VM3676:4 出错了 catch 里面的打印
  //hello
  ```

- 多个 await 命令后面的异步操作,如果不存在继发关系(互不依赖),最好让他们同时触发.

  ```javascript
  /**
  	*场景:
  	*上面代码中，getFoo和getBar是两个独立的异步操作（即互不依赖），被写成继发关系。这样比较耗时，因为只有getFoo完成以后，才会执行getBar，完全可以让它们同时触发
  **/
  let foo = await getFoo()
  let bar = await getBar()
  
  //写法
  let [foo,bar] = [foo, bar] = await Promise.all([getFoo(), getBar()]);
  
  ```

  

延伸问题:

Promise 和 async/await 的区别是什么?

```
promise是ES6，async/await是ES7,async/await 是基于 promise 实现的,写法更加优雅他们共同的目的都为了解决编程异步问题
他们的 reject 状态不一样,promise错误可以通过catch来捕捉，建议尾部捕获错误;async/await既可以用.then又可以用try-catch捕捉
```





了解完 promise 和 async/await 的用法后,我们将进入更深入的了解:

setTimeout async promise 执行顺序.

