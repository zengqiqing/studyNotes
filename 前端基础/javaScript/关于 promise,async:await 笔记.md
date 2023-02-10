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

    - 如果 then 返回的是一个promise,那么需要等这个promise执行完毕，promise如果成功， 就走下一个then的成功，如果失败，就走下一个then的失败

      

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

  - #### 以下代码说明 promise 的状态一旦决定后则不会再被修改

    promise对象代表一个异步操作，有三种状态：pending(挂起),fulfilled(已成功),rejected(已失败)。只有异步操作的结果，可以决定当前状态，其他操作无法改变这个状态，一旦状态凝固了就不会在变，会一直保持这个结果。

    ```javascript
    let p = new Promise((resolve,reject)=>{
        resolve('success1')
        reject('error')
        resolve('success2')
    })
    
    p.then(res=>{
        console.log('--res----',res)
    }).then(err=>{
        console.log('--err---',err)
    })
    
    //--res---- success1
    //--err--- undefined
    1.从上面的打印可以得出，promise的内部代码是同步的，一旦得出状态结果后，则不再改变其状态。
    2.无论状态结果为何，res和err 的回调打印，但是否有对应回调结果则由promise函数内部中首先执行状态来决定

   #### promise缺点：

    1.首先无法取消 promise，一旦新建他就会立即执行，无法中途取消。

    2.如果不设置回调函数，promise 内部就会抛出错误。

    3.当处于 pendding 状态，则无法得知目前进展到哪一步。

    

  - ### <font color='red'>promise 周边</font>

    #### promise什么时候搭配 new 来使用，搭配 new 和不使用 new的区别是什么？
    
    有启示性的构造器 promise 必须跟 new 一起使用，并且必须提供一个函数回调。这个回调是同步或者立即调用的。这个函数接收两个函数回调，用以支持 promise 的决议。---摘录《你不知道的 js》

    ```javascript
    下面是我的尝试👇
    const first = ()=>{
    	return Promise((resolve,reject)=>{
    		resolve(1111)
    	})
    }
    
    first()//浏览器报错，undefined is not a promise
    -------------------------------------------------------------
    
    const first = ()=>{
    	return new Promise((resolve,reject)=>{
    		resolve(1111)
    	})
    }
    
    first()//浏览器正常打印，1111
    -------------------------------------------------------------
    
    const first = Promise.reslove(1111) 
    first.then(res=>{
    	console.log(res) //1111
    
    })
    -------------------------------------------------------------
    
    var first = new Promise((resolve)=>{
        resolve(111)
    })
    
    first.then((res)=>{
        console.log(res)
    
    })//111
    -------------------------------------------------------------
    
  #### Promise.finally()
  
 不管promise最后的状态，在执行完then或catch指定的回调函数以后，都会执行finally方法指定的回调函数。
  
  ````javascript
  //语法
    promise
    .then(result => {···})
    .catch(error => {···})
    .finally(() => {···});
    ````
  

  
#### promise.all(同生共死)
  
  实战场景:Promse.all在处理多个异步处理时非常有用，比如说一个页面上需要等两个或多个ajax的数据回来以后才正常显示，在此之前只显示loading图标。
  
    - Pormise.all可以将多个 promise实例包装成一个新的实例方法。 promise.all结果返回顺序与请求所需的时间无关，与请求数组事件的排列顺序有关。就是这个语句:`Promise.all([func2(),func1()])`
  
    - 如果其中promise实例出现 reject情况,promise.all终止遍历并回调错误的内容信息,其余执行成功的promise实例内容是不会显示出来的因为promise内部发生错误了。如果 2 个接口都出现 reject情况,那么会先返回语法中排第一个的接口的错误信息出来

    - 如果参数中有一个promise失败，那么Promise.all返回的promise对象失败
  
    - 在所以的promise实例都执行成功的情况下，Promise.all 返回的完成结果都放置在数组上
  
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
      func() //[2222,1111]
      ```

      #### Promise.race(谁先回来返回谁)

        all是等所有的异步操作都执行完了再执行then方法，那么race方法就是相反的，谁先执行完成就先执行回调。all是回调一个固定的数组，race是谁先回调回来，就回调谁的数据类型和内容.

        如果其中一个接口出现 reject 的情况,那么还是看请求速度的,谁先回来就返回谁的result
  
        ```javascript
        var func1 = () => {
          return new Promise((resolve, reject) => {
            setTimeout(() => {
              resolve(1111);
            }, 500);
          });
        };
        var func2 = () => {
          return new Promise((resolve, reject) => {
            setTimeout(() => {
              resolve(2222);
            }, 500);
          });
        };
        var func = () => {
          return Promise.race([func1(), func2()])
            .then((res) => {
              console.log(res, "res");
            })
            .catch((err) => {
              console.log(err, "err");
            });
        };
        func(); //1111
      
        ```

  #### 延伸问题：

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

- 如何理解Async/await具有传染性

  ```javascript
  function getPicNum(num) {
      num = num + 1
      return num;
    }
    function getTotalPicNum(user1, user2) {
      const num1 = getPicNum(user1)
      const num2 = getPicNum(user2)
      return num1 + num2
    }
  
    async function getTotalPicNum(user1, user2) {
      const pic1 = await getPicNum(user1)
      const pic2 = await getPicNum(user2)
      return pic1 + pic2
    }
  	//如果屏蔽getTotalPicNum的方法,单纯执行getTotalPicNum,那么a打印出来的是就是结果值,但如果getTotalPicNum没有被屏蔽,其有调用getPicNum方法时,那么getTotalPicNum执行打印出来的a是一个promise。此处提现了async/await的传染性。
    const a = getTotalPicNum(1, 2)
    console.log(a, '--- a ---');
  
  //解决async/await 传染性的问题：
  //解读：使用async/await的原因是是为了解决异步问题，async function的本质就是对promise的封装。不想使用async/await 建议使用promise/then的方式。
  
  ```

  - 

  延伸问题:
  
  Promise 和 async/await 的区别是什么?
  
  ```
  promise是ES6，async/await是ES7,async/await 是基于 promise 实现的,写法更加优雅他们共同的目的都为了解决编程异步问题
  他们的 reject 状态不一样,promise错误可以通过catch来捕捉，建议尾部捕获错误;async/await既可以用.then又可以用try-catch捕捉
  ```
  
  
  
  了解完 promise 和 async/await 的用法后,我们将进入更深入的了解:
  
  setTimeout async promise 执行顺序.
  
  async/await  &&  try/catch的使用
  
  promise 与 try/catch的使用
  
  
  
  ### setTimeout 的故事
  
  #### setTimoutout的时间设定为 0 会发生什么事？
  
  下面代码是不是觉得很奇怪，为什么我的 setTimeout的时间设为 0 了，但他里面的 console 执行反而是最晚的呢？
  
  ```javascript
  settsetTimeout(()=>{
      console.log(1)
  },)
  
  console.log(2)
  console.log(3)
  
  //结果打印为 231
  
  原因：因为 js 是单线程的，单线程就意味着所有的任务都需要排队执行，前一个任务结束了，才会执行后一个任务，如果前任务耗时很长，后一个任务就不得不一直等待着。而浏览器的内核是多线程的，一个浏览器至少实现三个常驻线程：js 引擎线程，GUI 渲染线程，浏览器事件触发线程。
  
  1.js 引擎浏览器基于事件驱动单线程执行，js 引擎一直等待着任务队列中任务的到来，然后加以处理，浏览器无论何时都只有一个 js 线程在运行 js 代码。
  
  2.GUI渲染线程负责渲染浏览器界面，当界面重绘或回流时，该线程就会执行了，但注意 JS 线程和 GUI渲染线程是互斥的，当 JS 引擎执行时，GUI 线程就会被挂起，GUI 更新会被保存在一个队列中等 js引擎空闲时立即执行。
  
  3.事件触发线程，当一个事件被触发时，该线程会把事件添加到待处理队列的队尾，等待 js 引擎处理。这些事件可能来自 js 引擎当前执行的代码块，如 setTimeout，也可能来自浏览器内核的其他线程例如 click 事件，ajax 异步请求等，但由于 js 的单线程关系所有这事件都必须排队等 js 引擎处理完(当线程中没有执行任何同步代码的前提下才会执行异步)。那么当 js 引擎遇到定时器时，会将setTimeout中的 fn 这个函数放到任务队列中，当JS引擎空闲时并达到定时器的指定延迟时间时，才会将 fn 放到 js 引擎中执行。
  
  一句话总结：js 会先执行同步代码，空闲且到达定时器设定的时间后，fn 才被放到执行的任务队列中。
  ```
  
  
  
  #### 当 promise和setTimeout结合时，会发生什么事呢？
  
  ```javascript
  
  var first = async()=>{
      console.log(111)
      await async1()
      return new Promise((resolve,reject)=>{
          console.log(222)
          setTimeout(()=>{
              console.log(3333)
              resolve(4444)
          },0)
          resolve(5555)
      })
  }
  
  first().then(res=>{
      console.log(res)
  })
  var async1 = () =>{
      console.log('666')
  }
  
  //111，666,222，5555，3333
  
  1.因为first被调用了，所以先是进first函数内部，遇见 111。
  2.当碰到 awiat时，就会先执行 await 里面的内容，打印 666
  3.此时执行 promise，遇见 222。
  4.因为定时器在 promise里面，promise内部是同步执行的，所以当碰到定时器时，定时器会等同步执行完后才会执行定时器内部的代码。此刻他往下走了，碰到resolve。至此 promise 返回了状态结果。
  5.promise 的内部同步代码执行完毕，那么回过头去执行定时器。所以打印 3333
  为什么没有打印 444 呢，是因为 promise一旦状态改变，则不会改变其状态，也就是说已经 resolve 或者 reject 完， 其余的状态值都是无效的。
  ```
  
  

当 setTimeout,promise 遇上时，他们的执行时机是什么呢？当遇到 async/await 时呢？请查阅另外一篇文章

<a name="jump" href="../javaScript/关于宏观任务和微观任务的知识.md#test">跳转到深拷贝，浅拷贝的知识》》》》</a>

（按住command 时再点击才能跳转）

