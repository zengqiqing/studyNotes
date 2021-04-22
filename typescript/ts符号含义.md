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

  

- Typeof 

  ```
  
  ```

  

