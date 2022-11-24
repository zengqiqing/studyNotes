- `|`符号：联合类型，表示满足任意一个契约即可

  ```typescript
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
  ```

  

- `&` 符号：表示这个变量同时拥有所有类型所需要的成员，并且必须同时满足多个契约

  ```typescript
  //案例一
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
  func({a:'111',b:111,c:[1,2],d:111}) //ok
  func({a:'111',b:111}) //error，缺少c和d参数
  
  //案例二
  type F1 = (a:string,b:string)=>void
  type F2 = (a:number,b:number)=>void;
  
  let f:F1 & F2 = (a:string|number,b:string|number)=>{}
  
  f('1','2')//ok
  f(1,2)//ok
  f(2,'2')//error,没有与此调用匹配的重载...解决以上问题请看一下解答
  //案例二解答
  type F1 = (a:string,b:string)=>void
  type F2 = (a:number,b:number)=>void;
  type F3 = (a:number,b:string)=>void
  let f:F1 & F2 & F3 = (a:string|number,b:string|number)=>{}
  f(2,'2')//ok
  f('2',2) //ok
  
  //案例三
  type Foo = {
    age:number;
    name:string
  }
  type Bar = {
    age:number;
    name:string
  }
  type Baz = Foo & Bar
  const value:Baz = {
    age:1,
    name:'coco'
  }//ok
  
  type Foo = {
    age:number;
    name:string
  }
  type Bar = {
    age:boolean;
    name:string
  }
  type Baz = Foo & Bar
  const value:Baz = {
    age:1,
    name:'coco'
  }//error
  const value2:Baz = {
    name:'coco',
  }//error
  
  
  
  
  ```

- as符号：这个是类型断言

  将一个联合类型断言为其中一个。当ts不确定一个联合类型的变量到底是哪个类型时，我们只能访问此联合类型的所有类型中共有的属性或方法

  ```typescript
  interface Cat {
      name: string,
      run: () => void
  }
  
  interface Fish {
      name: string,
      swim: () => void
  }
  
  function isFish(animal: Cat | Fish) {
      if (typeof (animal as Fish).swim === 'function') {
          return '我是🐟'
      }
      return '我是🐱'
  }
  
  let tom: Fish = {
      name: 'tony',
      swim: () => { }
  }
  
  const isFishRight = isFish(tom)
  console.log(isFishRight, '--- isFishRight ----')
  
  ```

  

- declare : 你可以通过 `declare` 关键字来告诉 TypeScript，你正在试图表述一个其他地方已经存在的代码

  ```
  .d.ts文件是ts用来声明变量，模块，type,inteface或是一些第三方库没有支持ts等等内容，在.d.ts文件中声明变量或模块等内容后，其他地方可以不用import导入便可直接使用。但
  ```

  

- implements：希望在类中使用必须要被遵循的接口（类）或者别人定义的对象结构，可以使用`implements`关键字来确保其兼容性；

  

- in：可以安全地检查一个对象是否存在一个属性，通常用作类型保护。

  ```typescript
  interface A {
    x:number;
  }
  
  interface B {
    y:string
  }
  
  function doStuff (params:A|B) {
    if("x" in params){
      console.log('a 接口')
    }else{
      console.log('b 接口')
    }
  }
  
  doStuff({x:111}) //ok
  
  doStuff({y:'33'}) //error
  ```
  
  
  
- keyof：索引类型查询操作符，类似于object.keys，用于获取一个接口中key的联合类型

  ```typescript
  //keyof 操作符提取其属性的名称，该操作操作符可以用于某种类型的所有键，其返回类型是联合类型
  interface Person {
  name: string;
    age: number;
    location: string;
  }
  
  type K1 = keyof Person; // "name" | "age" | "location"
  type K2 = keyof Person[];  // number | "length" | "push" | "concat" | ...
  type K3 = keyof { [x: string]: Person };  // string | number
  
  //泛型中的keyof的使用
  interface arr = {name:'coco',age:3,size:'m'}
  const getInfo =<T extends object,K extends keyof T> (data:T,key:K):T[K]=>{
      return data[key]
  }
  getInfo(arr,size)//ok
  getInfo(arr,text)//error
  ```
  
  
  
- Typeof ：检查类型，用于基本数据类型

  ```typescript
  //类型的检查直接在代码里检查类型了
  function padLeft(value: string, padding: string | number) {
    if (typeof padding === "number") {
          return Array(padding + 1).join(" ") + value;
      }
      if (typeof padding === "string") {
          return padding + value;
      }
      throw new Error(`Expected string or number, got '${padding}'.`);
  }
  //这些typeof类型保护只有两种形式能被识别：typeof v === "typename"和typeof v !== "typename"，"typename"必须是"number"，"string"，"boolean"或"symbol"。 但是TypeScript并不会阻止你与其它字符串比较，语言不会把那些表达式识别为类型保护
  ```
  
- Instanceof：类型检查 。当我们使用typeof和 instanceof, ts会自动限制类型。instanceof用于引用类型。例如class,new ,但相对typeof，instanceof对基本类型不太管用

  ```typescript
  const test1 = new Array()
  console.log(test1 instanceof Aray)//true
  
  // 错误示范
  let num1 = 1
  console.log(num1 instanceof Number )
  ```
  
  
  
- extends : 这个extends关键词不同于在class后使用extends的继承作用，他是泛型内对泛型加以约束的关键词

  ```typescript
  type BaseType = string | number | boolean
   
  // 这里表示 copy 的参数
  // 传入的参数被约束为字符串、数字、布尔这几种基础类型
  function copy<T extends BaseType>(arg: T): T {
   return arg
  }
  
  const value = copy<string>('coco')//coco
  const value2<number[]> = copy([2])//error,Type 'string[]' does not satisfy the constraint 'BaseType'.
  ```

  extends不一定就是强制满足继承关系，而是检查是否满足结构兼容性。

  ```typescript
  //条件类型会以一个条件表达式进行类型关系检测，从而在两种类型中选择其一
  T extends U ? X:Y
  // 以上表达式的意思是：若 T 能够赋值给 U，那么类型是 X，否则为 Y
  ```

  

- in : 主要的作用是做类型的映射，遍历已有接口的key或者遍历联合类型。

- Exclude:将某个类型中属于另一个的类型移除掉

  ```typescript
  //如果 T 能赋值给 U 类型的话，那么就会返回 never 类型，否则返回 T 类型。最终实现的效果就是将 T 中某些属于 U 的类型移除掉。
  
  //例子1：以下例子中，逗号后面的内容就是要被移除的内容哦！
  type Exclude<T, U> = T extends U ? never : T;
  type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"
  type T1 = Exclude<"a" | "b" | "c", "a" | "b">; // "c"
  
  
  //例子2：
  type Omitc<T,K> = Pick<T,Exclude<keyof T,K>>
  type jihe = {size:string,age:number,name:boolean}
  type DefalutSizeMap = Omitc<jihe,'age'>//移除age
  const val1:DefalutSizeMap = {
    size:'big',
    name:false
  }//ok
  
  const val2:DefalutSizeMap = {
    age:2,
    size:'big',
    name:true
  }//error age属性应该是移除掉的
  
  //例子3：
  type Extractc<T,U> = T extends  U?T:never;
  type Result = 1|2|3|'error'|'success'
  type StringResult = Extractc<Result,string>
  const val1:StringResult = 'error'//ok
  const val2:StringResult = 'errrrr'//error,Result集合中不含有'errrrr'
  const val3:StringResult = 1//error,number类型被移除了
  ```

- Extra:

  ```typescript
  //语法：Extra<T,U>
  type numProps=
  ```
  
  
  
- readonly：

  ```typescript
  readonly与const的区别
  //const
  const aaa = 1323
  aaa = 444 //error ，const不允许变量重新赋值，使用let可以
  
  
  const foo: {
      readonly bar: number;
  } = {
      bar: 222
  }
  
  function imutateFoo(foo: { bar: number }) {
      foo.bar = 456
      // foo.bar = '456' //error，bar的类型是被定义为number的，这里设为string就会报错
  }
  
  imutateFoo(foo);
  console.log(foo.bar, 'xxxxxxxx'); //ok，因为readonly只是限定了类型，不会限定变量赋值。
  
  ```

  

- Infer:的作用是让ts自行判断，并将判断的结果存储到一个临时的名字中，并且只能用于extends 语句中，它与泛型的区别在于：泛型是声明一个“参数”,而 infer 是声明一个“中间变量”。infer语法限制如下：
  1.infer只能在条件类型的extends子句中使用。

  2.infer得到的类型只能在true语句中使用。

  ```
  ```

  

- 

- ReturnType<T>：获取函数类型的返回值

- Partial：将某个类型里的属性全部变为可选选项 `?`

  ```typescript
  interface someThing {
      sky:string;
      water:number;
      air:string
  }
  
  type person = Partial<someThing>;
  
  //Partial 的实现
  // type Partial <T> = {
  //     [p in keyof T] ? T[p]
  // }
  
  //那么P1就可以不需要必定传入someThing叙述的所有值啦！
  const P1: person = {
      sky:'1'
  }
  ```

- Pick:将某个类型中的子属性挑出来，变成包含这个类型部分属性的子类型

  ```typescript
  interface Todo {
      title:string;
      completed:boolean;
      desc:string
  }
  
  type TodoPreview = Pick<Todo,'title'|'desc'>
  
  //这里不能传入completed啦!
  const myTodo:TodoPreview = {
      title:'1',
      desc:'1'
  }
  ```

  

- Omit:使用T类型中除了K类型的所有属性,来构造一个新的类型(通俗理解就是除了x属性外，其他均属性组成一个新的类型)

  ```typescript
  interface Todo {
      title:string;
      completed:boolean;
      desc:string
  }
  
  type TodoPreview = Omit<Todo,'desc'|'title'>
  
  const myTodoEvent:TodoPreview = {
      completed:true,
      // title:'s'  title被排除在外了，所以这里不能写title
  }
  ```
  
  
  
- Record: 这是内部定义工具类型，record后面的泛型就是对象键和值的类型

  ```typescript
  
  //学习Record之前先来了解一下索引签名
  interface options {
    [key:string]:string|number|boolean
  }
  const data:options ={
    '1':'coco',
    '2':1
  } 
  
  console.log(data[1])//coco
  console.log(data['1'])//coco
  //当用作属性访问其中的键时，ts会将数字强制转换成字符串，跟js一样的哦
  
  //--除了使用索引签名外我们还可以使用内置record内置工具来实现---
  
  //解剖Record内置工具的实现
  type Record<K extends keyof any,T>={
    [P in K]:T
  }
  //P in xx的意思是将类型string进行遍历，也就是将any进行遍历，然后将属性值传入T类型中，T相当于value。
  
  //索引签名 vs Record
  // 1.索引签名参数类型不能为字面量类型或者泛型类型
  type User1 = {
    [key:'id']:string
  }//error
  
  type User2 =<Record><'id',string>//ok
  
  ```

  

  