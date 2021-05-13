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
  ```
  
  
  
- Typeof ：检查类型了

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
  
- extends : 这个extends关键词不同于在class后使用extends的继承作用，他是泛型内对泛型加以约束的关键词

  ```typescript
  type BaseType = string | number | boolean
   
  // 这里表示 copy 的参数
  // 传入的参数被约束为字符串、数字、布尔这几种基础类型
  function copy<T extends BaseType>(arg: T): T {
   return arg
  }
  ```

- in : 主要的作用是做类型的映射，遍历已有接口的key或者遍历联合类型。

- Exclude:

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

  

- 

- 

- 

  