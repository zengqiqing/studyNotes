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

  

