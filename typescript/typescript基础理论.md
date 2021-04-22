推荐插件：`TypeScript Auto Compiler` 每次保存ts文件就会自动生成对应的js文件

##### ts是什么？

ts是静态类型检查的弱类型语言，js是动态类型弱类型语言，如果js是一批野马，那么ts就是绑在野马身上的缰绳，让飞奔的js变得恬静而有规范。

##### 类型的基础

- 强类型语言：不允许改变变量的数据类型，除非进行强制性类型转换。（对变量类型的转换有严格的限制，不同类型的变量是不能相互赋值的）

- 弱类型语言：变量赋值没有约束，使用起来更灵活，但更容易出现低级bug。

##### 静态类型语言 & 动态类型语言

- 静态类型语言：在编辑阶段确定所有变量的类型。（对类型极度严格，能立即发现错误，运行时性能好，自文档化）

- 动态类型语言：在执行阶段确定所有变量的类型。（对类型宽松，bug不能立即发现，运行时性能差，可读性差）

  

##### ts的数据类型：

囊括了es6的数据类型以外，另外还增加了`void,any,never,元祖，枚举，高级类型`

##### ts与js的区别

ts在编译的时候就能发现类型的错误，js在运行的时候才能发现

变量的数据类型是不可以改变的。

##### ts的枚举类型

具体参考/Users/other/zqq/GitHub/ts_in_action/src/demo/enum.ts

### 声明空间

☝️：类型声明空间----- 类型声明空间用来当做类型注解的内容,但不能把它作为一个变量来使用，因为它没有定义在变量声明空间中。

```typescript
//定义类型声明空间
class Foo {}
interface Bar {}
type Bas = {}

//使用类型注解
let foo:Foo;
let bar: Bar;
let bas: Bas;

```

✌️：变量声明空间----变量声明空间包含可作用变量的内容。

```typescript
class Foo {} //提供一个类型
const somevar = Foo //提供一个变量Foo到class Foo{}的变量声明空间中
const someOthervar = 133
```

#### 文件模块

- 文件模块被称为外部模块，如果ts文件中含有`import` 或者 `export`，那么它会在这个文件中创建一个本地的作用域。

```typescript
//a.ts
export const foo = 111 //注意，如果a文件中没有export关键字，那么该文件中的变量方法会视为全局的，那么其他文件也定义同样名字的变量的时候就会声明重复的错误

//b.ts
import {foo} from './a.ts'
```

- 文件的导入导出

  ```typescript
  //1.使用export关键字导出一个变量或类型(语法一)
  export const someVar = 123;
  export type someType = {
    foo:string;
  }
  
  //1.1使用export关键字导出一个变量或类型(语法二)
  const someVar = 123;
  type someType = {
    type: string;
  };
  
  export { someVar, someType };
  
  //2.用重命名变量的方式导出(语法三)
  //a.ts
  const someVar = 123
  export {someVar as aDifferentName}
  
  //b.ts
  import {aDifferentName} from './a.ts' //ok
  import {someVar} from 'a.ts'//error
  
  //3.除了指定加载某个输出值，还可以使用整体加载，即用星号（*）指定一个对象，所有输出值都加载在这个对象上面
  
  //a.ts
  const someVar = 123;
  const kk = () => { }
  
  export {
      someVar,
      kk
  }
  
  // b.ts
  import * as foo from './a.ts';
  console.log(foo.someVar) //123
  
  ```

  ⚠️ 注意：

  - 



