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

  