```typescript
//数组的写法
let arr1:number[] = [1,2,3]
let arr2:Array<number> = [1,2,3]
let arr3:(number|string)[] = [1,2,'3']
let arr4:Array<number|string>=[1,2,'3']

//对象的写法，对象不能直接往对象中直接追加属性
let obj1 = {a:1,b:2}
obj1.a = 3//success,obj对象中存在a属性
obj1.c = 3//error,不能往对象中直接追加属性
let obj2:{a:number,b:string} = {a:1,b:'3'}



//Tuple的元祖的写法
//限制元素的个数和类型，索引下的元素值必须与对应的类型相符
//支持使用push扩增，扩增值类型必须是定义过的类型。同时不能通过索引访问扩增值
let tvalue = [number,string] = [1,'2']
let tvalue = [number,string] = [1,2]//error 索引1的值类型是string
tvalue.push(true)//error，布尔值没被定义过
tvalue.push(3)
console.log(tvalue)//[1,'2',3]
console.log(tvalue[2])//error,不能访问扩增值
console.log(tvalue[0])//1



//定义函数
function setName (name:string):string{
	console.log(name)
	return `这是函数的用法${name}`
}

const setName1 = (name:string):string =>{
	return `箭头函数的用法${name}`
}

const allCount1 = (...value,number[])=>{
  console.log(value)
}
allCount(1,2,3)//[1,2,3]

const allCount2 = (string:[])=>{
  console.log(['1','2','3'])//['1','2','3']
}

// 函数重载


//never
表示一个函数永远不存在返回值，never应该是void子集，因为void实际的返回值是undefined，而never连undefined都不支持。符合never情况有：当抛出异常情况和无限死循环。

let error = ():nerver=>{
  throw new  Errow('error')
}

let error1 = ():nerver=>{
  whild(true){}
}

//字面量型：
let num:1|2|3 = 1
console.log(num)//1
num = 4//error
num = 2//2

交叉类型（&）
//接口类型的交叉类型
type Aprops = {a:string}
type Bprops = {b:number}
type allProps = Aprops & Bprops
const Info1:allProps = {a:'coco',b:3}//正确写法
const Info2:allProps = {a:'coco',b:3,kk:2}//错误写法，因为不存在kk属性
const Info3:allProps = {a:'coco'}//错误写法，缺少了b属性

type Aprops = {a:string,c:number}
type Bprops = {b:number,c:string
type allProps = Aprops & Bprops
const Info1:allProps = {a:'coco',b:3,c:1}//erro
const Info2:allProps = {a:'coco',b:3,c:'coco'}//erro
//当交叉类型中出现重复属性，且属性类型不相同时，该属性为无效属性，该属性会变成never


//联合类型的交叉类型
type UnionA = 'px'|'em'|'rem'|'%';
type UnionB = 'vh'|'em'|'rem'|'pt';
type Union = UnionA & UnionB

const data:Union[] = ['em']//正确写法
const data:Union[] = ['em','px']//错误写法

//class的使用


//继承


//ts断言和类型守卫---告诉编辑器，放心用吧，我保证他是xx属性来的
//关键词：<> 和 as
function add(x:number,y?number){
  return x+(y as number)
}
add(1,2)

//ts断言和类型守卫 -- 非空断言
function add1(){
  const obj:{
    [key:string]:number
  } = {
    1:1,
    2:2
  }
	const key = 3
	const val = obj[key]//error,obj中不存在key为3的值
  const key1 = 2
  const val1 = obj[key1]!//ok，告诉浏览器相信我，这个值是没问题的，当然开发者要自己确保key是存在obj中的哦！
  }


//interface 和type的互相扩展
//interface 扩展 inteface
interface A {
  a:string
}
interface B extends A {
  b:number
}
const obj1:B={
  a:'coco',
  b:2
}

//type扩展type
type C = {a:string}
type D = C & {b:number}
const obj2:D = {
  a:'coco',
  b:2
}

// interface 扩展为type
type E = {a:string}
inteface F extends E {b:number}
const obj2:F = {a:'coco',b:2}

//type扩展为interface
interface G {a:string}
type H = G & {b:number}
const obj3:H = {a:'coco',b:3}

//interface重复定义
interface A {
  a:string
}

interface B {
  b:number
}

//泛型
//1.给key添加对应的类型
type aggregate<K extends keyof any,T> = {[P in K]:T}
type Size = 'small'|'default'|'number'
type SizeMap = aggregate<Size,number>
const kk:SizeMap = {
  small:2,
  default:3,
  big:2
}



```

