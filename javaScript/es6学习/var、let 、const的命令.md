## var、let 、const的命令

#### var的使用

- var存在变量提升的问题，主要概念：var变量的声明会被提升到函数的最顶部，也就是说，可以先使用 再声明

  ```javascript
  a = 1;
  var a ;
  console.log(a)//1
  b = 1 ;
  let b ;
  console.log(b)//引用报错，提示b不存在
  
  
  ```

#### let的使用

- let不允许在相同作用域内，重复声明同一个变量。var即使重复声明同一个变量，但后一个定义的会覆盖前一个声明的变量

  ```javascript
  var a =1
  var a = 3
  console.log(a) // 3
  let b =2
  let b = 4
  console.log(b)
  VM128874:2 Uncaught SyntaxError: Identifier 'b' has already been declared
  
  ```

  

**暂时性死区**

只要在块级作用域里面有let的定义变量，那么这个变量就‘绑定’在这个块级作用域里面，不受外部的影响， ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。也就是说在使用let声明变量前编写变量，那么这个变量是不可用的，这被称为暂时性死区.



#### const的使用

const声明一个只读的常量。一旦声明，常量的值就不能改变。

const一旦声明变量，就必须立即初始化，不能留到以后赋值。let只声明不赋值会是undefined，而const是直接报错

const命令声明的常量也是不提升，同样只在 所在的块级作用域内有效和存在暂时性死区只能在声明的位置后面使用。

const同样是不能重复定义



#### 什么时候使用let，什么时候使用const

const一般在require一个模块的时候用或者定义一些全局常量。而let是限制了变量的作用域，保证变量不会去污染全局变量。所以尽量将var改为用let。