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

    