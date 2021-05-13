- 发现一个问题，如果a.ts文件里面已经定义了名为obj的变量，此时在b.ts文件上也定义一个obj的变量时，会发生命名冲突，然后报错

  ```typescript
  //原因
  其实代码没问题，只是ts会将我们声明的变量，具名函数，class都放在全局作用域了，在生成js文件后，js文件里的变量，具名函数，class会跟ts文件的重复
  //解决方案 
  在文件顶部上方编写代码：export {} 即可避免命名冲突，相当于把整个文件当成一个模块来处理就不会报错啦~
  ```

  

1.要去了解一下Symbol具体使用场景。

2.函数类型- 重载没看透，还有this部分没深入看

3.访问一个越界的元素时,有没有什么写法，可以让x[2] 等于其他类型？

```typescript
let x: [string,number,number,boolean,number[]]
x = ['1',2,3,true,[1,2,3]] 
x[2] = 'coco'

console.log(x[2]) //Error, 索引为2类型是number，不是(string)类型

//4月16日回复
对于上面提出的使x[2] 等于其他类型的需求不合理，因为ts的目的就是为了限制变量的类型，如果要改变定义好的接口成员的属性就打破了ts原有的初衷

```

对ts中的常量枚举，外部枚举使用场景不太明确，不知道具体用在哪。当一个文件中存在`enum Char={a,b}`枚举后，另外一个文件定义别的枚举后`enum Evn{a,c}`这里a就会飘红报错，说已经存在了a

对ts中的object创建和使用不透测，不知道落地场景是什么?

```typescript
4月10日回复

可以参考 ’ts提问‘一章中创建boject的使用。
```



我无法理解TypeScript文档中的以下段落:"泛型函数的类型与非泛型函数的类型相似，首先列出类型参数，类似于函数声明:"最后一行是做什么的，为什么要使用它?

```typescript
function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: <T>(arg: T) => T = identity;
```



没搞懂类型兼容性里面讲的可选参数及剩余参数的例子

```typescript
function invokeLater(args: any[], callback: (...args: any[]) => void) {
    /* ... Invoke callback with 'args' ... */
}

// Unsound - invokeLater "might" provide any number of arguments
invokeLater([1, 2], (x, y) => console.log(x + ', ' + y));

// Confusing (x and y are actually required) and undiscoverable
invokeLater([1, 2], (x?, y?) => console.log(x + ', ' + y));
```



类型兼容：泛型

在这里，泛型类型在使用时就好比不是一个泛型类型。

👆这讲的是啥？

```typescript
interface NotEmpty<T> {
    data: T;
}
let x: NotEmpty<number>;
let y: NotEmpty<string>;

x = y;  // error, x and y are not compatible
```



implements还没搞太清楚。

