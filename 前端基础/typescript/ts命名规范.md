### 命名规范

1.类，枚举，接口，命名空间的命名使用帕斯卡命名法（PascalCase），成员使用驼峰命名法

1.1接口不要使用`I`做为前缀，枚举不要用`E`做为你的前缀 

2.变量和函数名使用驼峰命名法（camelCase）

3.尽量不显示使用不可用的值，例如`undefined`

```typescript
// Bad
let foo:{x:number,y:undefined} = {x:123,y:undefined}

//Good
let foo:{x:number,y?number} = {x:123}
```

4.使用箭头函数代替匿名函数表达式

5.函数需要带参数时才用括号将箭头函数的参数括起来，看下面的🌰

```typescript
❎ (x) => x + x
✅ x=>x + x
✅(x,y) => x+y
✅ <T>(x:T,y:T) => x === y
```

6.如果函数没有返回值最好用`void`