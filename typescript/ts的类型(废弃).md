```javascript
//原始类型

let bool:boolean = true;

let num:number = 123;

let str:string = 'abc'


//数组 --- 下面的种声明方式都是等价的,含义都是数组只能是number类型

let arr1:number[] = [1,2,3] 

/** 语法：变量:Array<number | string>

 \* 如果想为数组里面定义不同类型，那么需要用到联合类型(加多个|)的语法来使用,代表数组中所支持的类型有哪些
 \* 
 \* **/

let arr2:Array<number> = [1,2,3]

let arr2_1:Array<number | string> = [1,2,3,'4']



//元祖 --- 特殊数组，限定数据的类型和个数

/**

 \* 通俗点说就是限定了数组里面的每个索引下的内容的类型，一个萝卜一个坑，多出一个萝卜都会报错，萝卜放错坑也报错

 \* 元祖的越界范围：为数组push新元素，编辑器不报错

 \* **/

let tuple1:[number,string,number] = [2,'2',2]

let tuple2:[number,string] = [2,'2']

//以下方式不建议使用

tuple2.push(4);console.log(tuple2) //【2,'2',4】 编辑器不会报错，但是！！请看下面的console👇

// console.log(tuple2[2]) 这样是无法访问的，这个是因为ts不允许跨界访问，通常不建议这样做



//函数:

/**

 \* let add = (x,y) => x + y 这样写会报错的，因为没有给参数x,y赋予类型，那么相当于是隐式的any类型，容易出错,正确的写法应该要为参数添加类型定义，请看下面的写法👇

 *可以在箭头函数前添加类型值，意思是规范返回值的类型，但通常是可以省略的，除非特殊情况下才使用

 \*  **/

let add = (x:number,y:string) => x+y

add(1,'2');

let compute:(x:number,y:number) =>number;

compute = (a,b)=>a+b



//对象

/**

 \* let obj:object = {x:1,y:2}

 \* 如果想访问obj里面的属性：obj[x]那么是不能访问的，因为没有定义obj里面的value的类型，所以会报错

 \* **/

let obj:{x:number,y:string} = {x:1,y:'33'}


//symbol:具有唯一的值

let s1:symbol = Symbol()

let s2 = Symbol()


//undefined,null

/**

 \* 若值被赋值为undefined

 \* 可以为变量声明undefined和null,但变量被声明为undefined之后不能被赋值为其他类型，只能被赋值为本身

 \* 其他变量可以被设为null，但要到tsconfig中把strictNullChecks设为false,若不想设置strictNullChecks，那么要到num处设置联合类型：let num:number|undefined = 123;

 \* **/

let un:undefined = undefined;

let nu:null = null

num = undefined;



console.log(num,'----')



//void 操作符，让任何表达式返回undefined,确保返回值为undefined(比较少用)

let noReturn = () =>{}



//any:不指定任何变量就用any，跟js没有区别

let x



//never，永远不会有返回值，例如函数返回异常(比较少用)

let error = ()=>{

​    throw new Error("error");

}



//死循环函数，返回never类型(比较少用)

let endless = ()=>{

​    while(true){}

}
```

