## 关于this的指向问题

### this是什么？

this表示当前对象的一个引用，但在js中，this不是固定不变的，他会随着执行环境而改变的。

#### 普通对象，构造函数和普通的函数 的 this 指向都是怎样的呢？

 ```javascript
//普通对象中的this指向当前对象
myNewObject = {}
myNewObject.info = 'hello'
function myFunc = (){
	console.log(this.info,'111111')
	console.log(this,'2222222')
}
myNewObject.myFunc = myFunc
myNewObject.myFunc() 

//打印出来1111的值：’hello‘
//打印出来2222的值：’{info:'hello',myFunc:f}‘ 看，指向了myNewObject对象了
如果调用的myFunc会怎么样？
myFunc() 
//打印出来1111的值：undefined
//打印出来2222的值：Window对象
得出结论：在函数最初声明时，他的父对象是全局对象window,window对象中没有info值，所以打印this.info时得出undefined
若函数归属到一个对象中，那么他的this则指向归属的那个对象上

----------------------------------------------------------------------------------------------------

//普通函数的this指向window
具体例子可以参考上面提到的哦！！
function func (){
	console.log(this)
}
func()
//打印出来的值为window对象,也就是说，func的调用相当于是 Window.func()
如果函数里面有setTimeout呢，他的this又会指向哪里呢？
btn = {}
btn.onclick = function(){ 
    console.log(this,'11111')   
    setTimeout(function(){
        console.log(this,'22222')
    },500)
}

btn.onclick()
//打印出来1111的值：{onclick:f} 看，第一个this指向包含当前调用函数onclick函数的btn对象了
//打印出来的222的值：window对象
奇怪，为什么在setTimeout的this指向了window呢？？
唔，暂时没弄懂，但只要记住，传入定时器的函数，有哪个对象调用，我们不得而知的情况时，this就会指向window
----------------------------------------------------------------------------------------------------

// 构造函数的this指向new 出来的实例对象上
function Testfunc(name){
    console.log(this,'111111111')
    this.name = name
    console.log(this.name,'22222222')
}
先来试试不用new的效果，单纯是一个普通函数调用时，打印出来的this是怎样的。
var p1 = Testfunc('coco')
console.log(p1,'3333333')
//打印出来1111的值：window对象
//打印出来2222的值:coco
//打印出来3333的值:undefined

再来看看new出一个构造函数的效果
var p2 = new Testfunc('kevin')
console.log(p2)
//打印出来1111的值：{}一个空对象，因为这个this的打印是在this.name之上的，所以打印时他还是个空对象
//打印出来2222的值:kevin
//打印出来3333的值:Testfunc{name:'kevin'}指向了Testfunc new出来的实例对象p2

 ```

##### 结合上面的普通函数与setTimeout的爱恨情仇，那么提出了个疑问，我要怎么样才能让setTimeout的this指向上层的函数上呢？

```javascript
//方法A (这种方法最常用)
var btn = {}
btn.onclick = function (){
	var that = this;
	setTimeout(function(){
		console.log(that)
	},0)
}
btn.onclick

//方法B (使用call)
var btn = {}
btn.onclick = function(){
	var that = this;
	function fn(){
		console.log(this);
	}
	setTimeout(function(){
		fn.call(that)
	},0)
}
btn.onclick()

//方法C (使用apply)
var btn = {}
btn.onclick = function(){
	var that = this;
	function fn(){
		console.log(this);
	}
	setTimeout(function(){
		fn.apply(that)
	},0)
}

btn.onclick()

//方法D (使用bind) 这种方法最好用
var btn = {};
btn.onclick = function(){
	setTimeout(function(){
		console.log(this)
	}.bind(this),0)
}
btn.onclick()
```

#### apply ,call ，bind的用法

call / apply / bind 是Function.prototype上的三个方法，只有函数才有这些方法，最终的返回值是你调用的方法的返回值，若该方法中没有返回值，则返回undefined。

作用：改变函数执行时的this指向，可以借助他们实现继承

具体例子参考：https://www.runoob.com/w3cnote/js-call-apply-bind.html

```javascript
// call:第一个为函数上下文也就是this，其余参数是一个个传入的
func.call(thisArg,param1,param2,....);
- call返回的是fn执行结果

// apply：第一个为函数上下文也就是this，其余参数传入数组
func.apply(thisArg,[param1,param2,....])
- apply返回的是fn执行结果

/*
	* bind的参数传入方式跟call的比较像，第一个为函数上下文也就是this，其余参数都是一个个传入，不同的是bind完返回的是一个新的函数，需要调用时再自己拿来用
*/

语法：fun.bind(thisArg,param1,param2,...)()

 调用方式 1 ： var a = xx.bind('参数1','参数2');  a()
	调用方式 2 ： var a = xx.bind('参数1','参数2')() //立即调用
					
返回是fn的拷贝，并拥有指定的this值和初始参数
```

#### call 和 apply 和 bind 的区别，什么时候用call, 什么时候用apply ?

````javascript
// 1.传参方式不同，call的参数是一个个传入的，而apply的第二个参数起，是用数组传入的
// 2.call是apply的语法糖
// 3. bind是返回一个新函数，供以后调用，而apply 和 call是立即调用

什么时候用call, 什么时候用apply 
// call采用不定长的参数列表，而apply使用一个参数数组，当你参数不可预估时用apply,可预估时用call
````

#### 实现 call , apply ,bind 的方法

https://zhuanlan.zhihu.com/p/92786246

https://segmentfault.com/a/1190000014342422

#### 总结：

得出结论：

- 所有的this关键字，在函数运行时，才能确定它的指向。
- this所在的函数由哪个对象调用，this就会指向谁
- 当函数执行时，没有确定的调用对象时，则this指向window

