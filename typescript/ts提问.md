
1.要去了解一下Symbol具体使用场景。

2.函数类型- 重载没看透，还有this部分没深入看

访问一个越界的元素时,有没有什么写法，可以让x[2] 等于其他类型？

```typescript
let x: [string,number,number,boolean,number[]]
x = ['1',2,3,true,[1,2,3]] 
x[2] = 'coco'

console.log(x[2]) //Error, 索引为2类型是number，不是(string)类型
```

对ts中的常量枚举，外部枚举使用场景不太明确，不知道具体用在哪。

对ts中的object了解不透测，不知道落地场景是什么



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



交叉类型没看懂！