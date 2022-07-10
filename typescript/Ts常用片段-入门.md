- for循环练习：

  ```typescript
  let arr = [1,2,3]
  for(let i:number = 1;i< arr.length; i++){
  	console.log(i,'--- i ----')
  }
  ```

- 创建数组：

  ```typescript
  //语法一（推荐）
  let arr:string [] = [] //此处表示字符串类型的数组（只能出现字符串类型）
  
  //语法二（不推荐，因为繁琐）
  let arr:string [] = new Array;
  ```

- 获取数组长度

  ```typescript
  let food:number[] = []
  food = [1,2,3]
  let len1 = food.length
  console.log(len1) //ok
  
  let coco:string
  coco = "1111"
  let len2 = coco.length//ok
  
  let kevin:boolean
  let len3 = kevin.length//error,布尔值没有length属性
  ```

  

- 创建一个对象

  ```typescript
  interface Person {
      name:string,
      age:number
  }
  let person = {
      name:'coco',
      age:11
  }
   console.log(person,'-- person --')
  
  //为对象新增一个新的属性
  interface Person {
      name:string,
      age:number,
      [x:string]:any
  }
  
  let p1 = {}
  let p2 = p1 as Person
  p2.sex = 'ddd'
  /*之前一直没弄懂为什么直接定义一个空对象，然后给里面赋属性会报错，这是因为foo的类型忒累为{},具有0属性的对象，不能直接添加key，但可以要通过类型断言来避免问题（👆上面那个例子鸭） */
  ```



- 为数组中的push新的内容

  ```typescript
  let song:number[] = [1,2,3]
  song.push(4)
  console.log(song.length)
  console.log(song[3])
  ```

- 限定函数传入的参数

  ```typescript
  //使用场景：限定两组参数，两组参数都支持timeType参数的传入，但若传入ISecond的参数，那么传入IDay参数就会报错。
  //秒
  interface ISecond {
      timeType: string,
      secondTime: number
  }
  
  //天
  interface IDay {
      timeType: string,
      startTime: number
      endTime: number
  }
  
  interface ReloadSecondAndDay {
      (x: ISecond): ISecond;
      (x: IDay): IDay;
  }
  
  const PropsInterface: ReloadSecondAndDay = (x: any) => x;
  
  PropsInterface({timeType:'aaa',secondTime:111})
  PropsInterface({timeType:'aaa',secondTime:111,endTime:222})//报错
  ```

  

- 记录限制组件参数传入：

  ```typescript
  interface IA {
    a: string;
  }
  
  interface IB {
    b?: number;
    c: string;
  }
  
  //Exclude<keyof T, keyof U> 找出两组参数的不相交的参数，若有查找出结果那么返回结果，否则不返回值
  export type Without<T, U> = {
    [P in Exclude<keyof T, keyof U>]?: never;
  };
  
  //a参数或b参数约束类型为对象，若传入参数为对象，那么执行Without对参数进行校验，若没有返回值，那么直接返回传入的参数，不作校验
  export type XOR<T, U> = T | U extends object
    ? (Without<T, U> & U) | (Without<U, T> & T) //这里还是不太明白
    : T | U;
  
  const A = (props: XOR<IA, IB>) => {
    return <div>1</div>;
  };
  
  const B = () => {
    return <A c={'ddd'} />;
  };
  ```

- 使用接口定义函数传入的参数值（适用于函数传参）

  ```typescript
  // & 和 | 操作符
  
  // TC类型的变量的键只需包含ab或bc即可，当然也可以abc都有
  interface IA {
      a:string,
      b:number
  }
  
  type TB = {
      a:string,
      c:number[]
  }
  
  type TC = IA | TB
  type TD = IA
  
  const func = (params:TC) => {}
  
  func({a:"111",c:[1,2]}) //ok
  func({a:"111",d:233}) //no d不存在于接口IA和IB中
  
  //-------------------------------------------------------------
  
  interface IA {
      a:string,
      b:number,
      d:number
  }
  
  type TB = {
      a:string,
      c:number[]
  }
  
  type TD = IA & TB
  
  const func = (params:TD) => {}
  
  func({a:'111',b:111,c:[1,2],d:111})
  ```

  

- 记录一个使用类型断言的场景

  ```typescript
  //错误的写法：-----用ts这样子写的话会飘红，提示	Property 'name' does not exist on type '{}'.(大致意思是obj对象中不存在属性name)
  let obj = {}
  obj.name = 'coco'
  
  //正确的写法：-----使用类型断言解决
  interface Foo {
      id: number;
      name: string
  }
  let obj = {
      id: 1
  } as Foo
  obj.name = "coco"
  console.log(obj, '---- obj ---');
  
  //这样写也是可以的
  let obj = {} as Foo
  obj.id = 111
  obj.name = 'kevin'
  
  console.log(obj, '---- obj ---');
  
  ```

  //接上一题，如果想扩展对象里面的属性如何扩展呢？？

  ```typescript
  interface Foo {
      id: number;
      name: string,
      [x: string]: string | number //支持扩展key是string或是number类型的属性
  }
  let obj = {} as Foo
  obj.id = 222
  obj.name = 'kevin'
  obj.coco = 22
  console.log(obj, '---- obj ---');
  ```

  

