由于篇章比较长，但实际介绍的是反引号的使用，所以不长篇记录，仅做小记

### 反引号

反引号是es6的特性之一，引入字符串，${变量名}，前后不需要加号连接。甚至可以在反引号里面调用函数哦~

```javascript
//es5的写法：
var a = {name:'coco',age:22}
var b = '是最可爱的人'
var c = a.name + b

es6写法：
var a = {name:'coco',age:22}
var b = '是最可爱的人'
var func = getName = ()=>{
	return 'lisa'
}
var c = `${a.name}${b}`
var d = `${func()}${b}`
```

### 补全（padStart( ) , padEnd( )）

这个归属于es7的语法，常见的使用场景是字符串首位补全字符串,例如时间展示，不满10时补0

```javascript
var a = '1'
var b = a.padStart(2, '0')
console.log(b)
```

### 消除空格 trimStart()，trimEnd() 

这个归属于es9，使用类比trim( )，但trim是可以移除字符串的首位处的空白字符，他们使用的场景是消除字符串首位的空格，返回的是新的字符串，不会修改原字符串

```javascript
const s = '  abc  ';
s.trim() // "abc"
s.trimStart() // "abc  "
s.trimEnd() // "  abc"
```

### rest的使用

这个记录在解构赋值一文中，因为这个方法很常用，所以特地记录一下。通过解构并且指定某移除属性后，得出的rest就是就是移除后的新对象。他不会影响源对象

```javascript
var obj1 = {id:1,name:"coo",age:22}

function getNewObj (){
	const {id,...rest} = obj1
	return rest
}
var obj2 = getNewObj()

console.log(obj1) // {id:1,name:"coo",age:22}
console.log(obj2) // {name:"coo",age:22}
```

还可以结合箭头使用哦！但感觉比较少用到啦

```javascript
const headAndTail = (head, ...tail) => [head, tail];

headAndTail(1, 2, 3, 4, 5)
// [1,[2,3,4,5]]
```

### 链判断运算符

`?.`的用法： 避免读取对象内部的某个属性时因属性不存在而报错。该方法来源于es2020。当读取不到时，不会直接报错，而是报undefined

```javascript
var obj = {kiki:{age:22}}
console.log(obj.kiki.name) //报错啦

console.log(obj?.kiki?.name) //undefined
```

`??`的用法：判断null和undefined的方法。如果为左边的值为null或undefined，那么右边的才执行，否则只执行左边的代码。(个人感觉比较鸡肋)

```javascript
var a = null
var b = a??'cc'
console.log(b)// cc

var a = 0
var b = a??'cc'
console.log(b) //0

var a = false
var b = a??'cc'
console.log(b) //false
```

`||=`的用法：为变量或属性设置默认值 (个人感觉比较鸡肋)

```javascript
//if判断
let a;
if(!a){
	a = 3
}
//三元运算
a = a?a:3
//运算符
a = a||3
//使用||= 
a||=3
console.log(a) //3
```

