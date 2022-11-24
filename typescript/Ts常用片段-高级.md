```typescript
// find的写法
interface IArray {
    name:string,age:number,isGirl:Boolean
}
// = {name:string,age:number,isGirl:Boolean}[]
type filterArray = {name:string,age:number,isGirl:Boolean}
const xiaojiejie:IArray[] = [{name:'coco',age:12,isGirl:true},{name:'lili',age:33,isGirl:true}]

function  myFunc5(params:IArray[]) {
    const newArr = params.find((item)=>{
            return item.name === 'coco'
    })
    return newArr
}
// console.log(myFunc5(xiaojiejie))

// push的写法
const pushData:IArray[] = [{
    name:'lily',
    age:12,
    isGirl:true
}]
const pushParams:IArray = {
    name:'kevin',
    age:39,
    isGirl:false
} 
pushData.push(pushParams)

function pushFunc(array:(string|number)[],...items:number[]) {
    items.forEach((item:number)=>array.push(item))
}

let a:(string|number)[] = ['3','4',4]
pushFunc(a,1,2)

// map的写法
interface MAPFUNC {
    id:string,
    [propname:string]:any
}
function mapFunc(array:MAPFUNC[]){
    const cbData = array.map(item=>item.id)
    return cbData
}
let mapFuncB = [{id:'1',name:'coco'}]
let mapFuncData = mapFunc(mapFuncB)

// filter写法
interface FILTERFUNC{
    id:number,
    name:string,
    age:number
}

function filterFunc(array:FILTERFUNC[],key:number) {
    return array.filter(item=>item.id===key)
}
let filterFuncA = [{id:1,name:'coco',age:22},{id:2,name:'linko',age:12}]
const filterData = filterFunc(filterFuncA,1)
console.log(filterData,'--- filterData ---');


//将必填的属性改变为可选
type PartialByKeys<T,K extends keyof T> = {
  [P in K]?:T[P]
}&Pick<T,Exclude<keyof T,K>>

type Simplify<T>={
  [P in keyof T]:T[P]
}

type User = {
  id:number;
  name:string;
  age:number
}
type U1 = Simplify<PartialByKeys<User,'id'|'name'>>

const value:U1 = {
  age:2
}//ok
//解读一：U1:上面的U1解读下来入下方
type U1 = {
  id?:number|undefined;
  name:string;
  age:number
}

//解读二：为什么一定要套多一个Simplify，PartialByKeys不也可以解决需求吗？
//答：使用Simplify可以让我们更清晰地知道错误的点在哪里，请对比一下他们的报错信息。使用了simlify会更清晰知道错误点再哪。而仅使用PartialByKeys提示未够清晰，并且他也是有提示我们需要做去做进一步的分析哦。
1.仅使用PartialByKeys未使用Simplify的报错👇
Type '{ id: number; name: string; }' is not assignable to type 'U1'.
  Property 'age' is missing in type '{ id: number; name: string; }' but required in type 'Pick<User, "name" | "age">'

2.使用Simplify的报错👇
Property 'age' is missing in type '{ id: number; name: string; }' but required in type 'Simplify<PartialByKeys<User, "id">>'.


```

