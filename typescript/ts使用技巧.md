可选参数和可选属性

- 使用了`--strictNullChecks`，可选参数会被自动地加上`| undefined`

```typescript
function f(x: number, y?: number) {
    return x + (y || 0);
}
f(1, 2);
f(1);
f(1, undefined);
f(1, null); // error, 'null' is not assignable to 'number | undefined'
```

- 可以使用联合类型来将值定义为null或undefined类型

  - 可以设置typescript.json文件的strictNullChecks,将null和undefined的赋值错误解决掉

  ```typescript
  let s = "foo";
  s = null; // 错误, 'null'不能赋值给'string'
  let sn: string | null = "bar";
  sn = null; // 可以
  
  sn = undefined; // error, 'undefined'不能赋值给'string | null'
  
  //注意，按照JavaScript的语义，TypeScript会把null和undefined区别对待。 string | null，string | undefined和string | undefined | null是不同的类型
  ```

- 使用一组有限的字符串字面量

  - 一个索引签名可以通过映射类型来使索引字符串为联合类型中一员

    ```typescript
    type Index = 'a'|'b'|'c';
    type FromIndex = {[k in Index]?:number};
    const good:FromIndex = {b:1,c:2}; //ok，符合联合类型中的一员
    
    const bad:FromIndex = {b:1,c:2,d:3} //error，因为d变量没有被定义
    ```

- 解决cb回调函数报错：

  ```typescript
  declare function isfunction(cb:unknown):cb is Function;//使用unknown定义
  function f20(cb:unknown){//结合判断条件
      if(cb instanceof Error){
          cb;
      }
      if(isfunction(cb)){
          cb;
      }
  }
  ```

- 如何减少接口中的属性重复定义：除了可以利用extends和’&‘以外，还可以利用in 或pick

  ```typescript
  interface State {
      userId:string;
      pageTitle:string;
      recentFiles:string[];
      pageContents:string;
  }
  
  interface TopNabState{
      userId:string;
      pageTitle:string;
      recentFiles:string[];
  }
  
  // 1.利用in符号将属性中的类型进行映射组成一个新的接口哦！
  type TopNavState = {
      [k in 'userId'|'pageTitle'|'recentFiles']:State[k]
  }
  
  //2.利用pick方法更简洁哦！
  type TopNavState2 = Pick<State,'userId'|'pageTitle'|'recentFiles'>
  const val:TopNavState2 = {
      userId:'string',
      pageTitle:'string',
      recentFiles:['string'],
      name:'coco'//error
  }
  
  ```
  
  

