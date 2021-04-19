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

##### 对象类型接口

- 鸭式辨型法--（动态语言风格）有一只鸟，叫起来像鸭子，走路像鸭子，那么这只鸟就可以被认为是一只鸭子

  ```typescript
  interface List{
      id:number;
      name:string;
  }
  
  interface Result {
      data:List[]
  }
  
  const render = (results:Result) =>{
      const {data} = results;
      data.forEach((value)=>{
          console.log(value.id,'----- 对象接口 ----',value.name);
      })
  }
  
  let results = {
      data:[
          {
              id:1,
              name:'coco',
              sex:'male'//这个sex没有被定义类型，但在这里没有报错，这是因为传入的对象满足接口的必要条件，那么即使有多余的属性也不会报错
          },
          {
              id:2,
              name:'kevin'
          }
      ]
  }
  
  render(results)
  ```

  ##### 接口（对象类型接口、函数类型接口）

  简单函数类型接口：

  ```typescript
  //语法
  //回顾之前：使用变量定义函数的写法
  let add1:(x:number,y:number) => number;
  
  interface add2 {
      (x:number,y:number):number
  }
  
  
  ```

  混合型函数接口（一个接口既可以定义一个函数，也可以像对象拥有属性和方法）：

  ```
  
  ```

  

  