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

```

