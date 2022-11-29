#### 误更改对象值问题（涉及内存地址问题）

场景：这个问题我经常会忽略掉，当一个对象作为一个实际参数传递给函数使用时，有时候为了方便或者为重组一个新的数据时，通常会直接修改实参的key和value值，或者往里面新增key & value。确实这样非常方便，但有没有想过这样会误改到存在函数外部的作为函数的实参的params对象呢？万一这个params还有别的用处呢？

```javascript
//案例
function test(person) {
  person.age = 26
  person = {
    name: 'coco',
    age: 18
  }
  return person
}
const p1 = {
  name: 'kevin',
  age: 19
}
const p2 = test(p1)
console.log(p1)
console.log(p2)
//打印p1结果为：{name: "kevin", age: 26}
//打印p2结果为：{name: "coco", age: 18}
```

问题分析：

问: 为什么在函数里修改实参对象时会直接修改到定义在外部的对象呢？

答：其实无论是在函数内部修改还是在脚本的全局某个地方修改对象属性是没有区别的。因为他们指向的都是同一个内存地址，所以修改key和value值时，无论是在函数体内外，都会修改到同一个内存地址上的内容

案例拓展：

```javascript
// 举个🌰1
const obj = {
	id:1,
	name:'coco'
}
obj.id = 3
console.log(obj) //{id:3,name:'coco'}主动变更其value

//举个🌰2
var obj1 = {id:1,name:'coco'}
var obj2 = obj1
obj2.id =3
console.log(obj1)//{id:3,name:'coco'}不要被赋值迷惑了，因为他们指向的都是同一个内存地址，所以当obj2修改时，obj1也会被改到

//举个🌰3
var obj1 = {id:1,name:'coco'}
obj1 = {sex:'boy'}
console.log(obj1)//{sex:'boy'} 不要跑偏了，对，没错，都是指向同一个内存地址，但这是重新赋值了，前值被后面的值给覆盖了！

//举个🌰4
var obj1 = {id:1,name:'coco'}
var func = (obj1)=>{
     obj1.id = 3
     return obj1
}
var p1 = func(obj1)
console.log(p1)//{id: 3, name: "coco"} 不要被函数迷惑了，其实函数里的obj1.id = 3跟🌰1的修改是一样的！！！

```

问：为什么我会犯这样的错误呢？这是因为我图方便，想重组数据后提供别处使用。有想过解决方案吗？

答：如果是对象的话可以使用解构赋值解决这个问题。如果是数组的话就应该使用map的方式啦！~~~

```javascript
//对象
var obj1 = {id:1,name:'coco'}
var obj2 = {...obj1,id:3}
console.log(obj1)//{id: 1, name: "coco"}
console.log(obj2)//{id: 3, name: "coco"}

//数组
var arr = [{id:1,name:'coco'},{id:2,name:'kevin'}]
var brr = arr.map(v=>{
    var obj = {sex:'boy',age:22}
    if(v.id===1){
        return {
            ...v,
            name:'yuki',
            ...obj
        }
    }else{
        return {
            ...v,
            ...obj
        }
    }
})

console.log(brr) //[{id: 1, name: "yuki", sex: "boy", age: 22},{id: 2, name: "kevin", sex: "boy", age: 22}]
console.log(arr) //[{id: 1, name: "coco"},{id: 2, name: "kevin"}]
```

### 浅拷贝的限制所在就是：它只能拷贝一层对象。如果有对象的嵌套，那么浅拷贝将无能为力

```javascript
//🌰
let arr = [1, 2, {val: 4}];
let newArr = arr.slice();
newArr[2].val = 1000;

console.log(arr);//[ 1, 2, { val: 1000 } ]
```



