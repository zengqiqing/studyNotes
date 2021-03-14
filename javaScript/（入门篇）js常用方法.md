## js常用方法

### 以下方法使用于Object

<font color='red'>判断为空对象：**Object.keys(userObj).length**</font>

```javascript
let userObj = {id:1,name:'coco'}
if(Object.keys(userObj).length){
    console.log('当前对象里有内容')
}
```



<font color='red'>**如何获取对象里面的值，确保不会因为没有值而不报错呢？**</font>

```javascript
var obj = {ids:{name:'coco'}}
console.log(obj?.id?.name) // undefined
console.log(obj.id.name) //报错

```



<font color='red'>**如何快速获取对象中的某几个属性：**</font>

```javascript
// 1.当要取的对象内容比较多的时候（取反），下图排除a以外的属性都要获取，组成一个新的对象
var obj = {a:1,b:2,c:3,d:5}
var {a,...rest}=obj
console.log(rest)
//{b:2,c:3,d:5}
// 2.正向取属性，在有多个属性的对象中，只获取a,c两个属性，组成一个新的对象
const object = { a: 5, b: 6, c: 7 }
const picked = (({ a, c }) => ({ a, c }))(object)
console.log(picked) // { a: 5, c: 7 }

```



<font color='red'>**判断某属性是否存在于对象中**</font>

```javascript
var obj = {id:1}
var b = 'name' in obj
console.log(b)

```



<font color='red'>**判断一个变量是否为数组**</font>

```javascript
function isArray(arr){
    return Object.prototype.toString.call(arr) === '[object Array]';
}
```



<font color='red'>**替换对象的属性，值不被变更。**</font>

```javascript
Var postData = {appletName:’111’,name:’coco'}
Var newPostData = JSON.parse(JSON.stringify(postData).replace(/appletName/g,"appId"));
console.log(newPostData)
//{appId:’111’,name:’coco'}

```



<font color='red'>**tofiexd（2）**</font>保留2位小数方法适用于number类型，使用前，请先转换成number类型

**Number() 或者****parseInt()** 字符串转number类型：



<font color='red'>**join( )**数组转字符串****</font>

```javascript
let array = ["a","b","c"]
array.join()
//结果："a,b,c,”
```



<font color='red'>**将字符串中的逗号替换成/**</font>

```javascript
var test = "刘希日记,宝宝的瓜"
console.log(test.replace(/,/g,"/“))
//刘希日记/宝宝的瓜
```



<font color='red'>**去除数组中的空数组**</font>

```javascript
var obj={100:[]};for(var key in obj){ if(obj[key] ==''){ delete obj[key] } } console.log(obj)
```



<font color='red'>**什么是类数组，将类数组转换成数组**</font>

数组的length属性，当新的元素添加到列表中的时候，其值会自动更新。类数组对象的不会。

将类数组，转换成数组

`var trueArr = Array.prototype.slice.call(arrayLike)`



<font color='red'>**Object.assign 合并对象：**合并2个对象成为一个新的对象:</font>

![EB86DD7A-F211-4B4C-874E-0F72522ED3FE](/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.pN2gnB/EB86DD7A-F211-4B4C-874E-0F72522ED3FE.png)



<font color='red'>**for in / for of / forEach**</font>

for in：用于对对象属性名进行遍历，虽然也可以遍历数组但不推荐。 在遍历属性多的对象时候，for-in效率低下：for-in内部偷偷调用了Object.keys()

```javascript
let obj = {a: '1', b: '2', c: '3', d: '4'}
for (let o in obj) {
console.log(o) //遍历的实际上是对象的属性名称 a,b,c,d

console.log(obj[o]) //这个才是属性对应的值1，2，3，4

}
```

for of：用于对数组内的值进行遍历的，是es6新增的语法

循环一个数组：

```javascript
let arr = ['China', 'America', 'Korea']
for (let o of arr) {
console.log(o) //China, America, Korea
}
```

循环一个字符串：

```
let str = 'love'for (let o of str) {
console.log(o) // l,o,v,e
}
```

循环一个可枚举型的对象

```
// 如果我们按对象所拥有的属性进行循环，可使用内置的Object.keys()方法
//循环key值 let obj = {a: '1', b: '2', c: '3', d: '4'}
for (let o of Object.keys(obj)) {
console.log(o) // a,b,c,d
}
// 如果我们按对象所拥有的属性值进行循环，可使用内置的Object.values()方法 //循环value值
let obj = {a: '1', b: '2', c: '3', d: '4'}
for (let o of Object.values(obj)) {
console.log(o) // 1,2,3,4
}

```

forEach：其实用法和for遍历的用法一样，是es5的语法

for...in 语句用于对数组或者对象的属性名进行循环操作。for...in适合遍历对象

for ... in 循环中的代码每执行一次，就会对数组的元素或者对象的属性进行一次操作。



<font color='red'>**符号|的使用**</font>

符号 || 是用来做判断，‘或’的意思。 符号 | 是 二进制之后相加得到的结果， |0 就是取整的方法。通用应用于分秒换算

<font color='red' fontWeight='bold'>a为0时代表的是一种状态，那么我要判断a不为空不为null但能为0时，执行下一步的操作该如何编写更优雅？</font>

```javascript
var a = null
if(a||a===0){
 console.log('进入')
}else{console.log('退出')} // 退出
```



<font color='red'>**Number.isFinite()**</font> 检测一个数值是否有限（即不是Infinity）此方法不能用于字符串，只适用于类型为nuber类型或infinity无限大

Number.isFinite('foo'); // false

Number.isFinite('15'); // false

<font color='red'>**Number.isNaN()**</font>用来检查一个值是否为NaN。

Number.isNaN(NaN) // true

Number.isNaN(15) // false

<font color='red'>**Math.ceil**</font> 向上取整（四舍五入）

<font color='red'>**Math.floor**</font> 向下取整 (四舍五入)

Math.abs(-1)//1 取绝对值

<font color='red'>**trim()**</font> 去除字符串头尾的空格.

var str = " Runoob "; console.log(str.trim()) //Runoob



### 常用数组方法总结：（以下方法适用于**数组**）

<font color='red'>**判断当前值是否为数组：**</font>

```javascript
let arr = [1,2]

arr instanceof array && typeof(arr) **instanceof：**
```

原理：运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

语法：

instanceof Array 判断是否为数组

instanceof Object 判断是否为对象，注意，array的原始类型也是object

<font color='red'>**concat()**拼接法：</font>

用于连接两个或多个数组。该方法不会改变现有数组，只是返回被连接数组的副本,concat为浅复制，复制后的引用都是指向同一个对象的实例，彼此之间的操作会互相影响

```javascript
var stringValue = "Hello ";
var result =stringValue.concat("world");
alert(result);//"Hello world”
Es6拼接写法：
let aaa = [‘hello’,true,7]
let bbb = [1,2,…aaa]
console.log(bbb)
//[‘hello’,true,7,1,2]
```

<font color='red'>**去重**</font>

<font color='red'>es6 **set方法**：</font>

```javascript
function unique10(arr){
 //Set数据结构，它类似于数组，其成员的值都是唯一的
 return Array.from(new Set(arr)); // 利用Array.from将Set结构转换成数组
}
```

<font color='red'>**数组对象去重**</font>

```javascript
/* 数组去重 */
arrayNewSet = (array) => {
  let temp = {};
  return array.reduce((cur, next) => {
    if (!temp[next.id]) {
      temp[next.id] = true;
      cur.push(next);
  	}
	return cur;
}, []);
};
```

去重方法二：

```javascript
var newArr = arr.reduce(function (prev, cur) {
    prev.indexOf(cur) === -1 && prev.push(cur);
    return prev;
},[]);

```



<font color='red'>**push推送法**（）</font>向数组后追加元素

push 的定义是：向数组的末尾添加一个或更多元素，并返回新的长度。该方法会改变数组的长度。

```javascript
var a = "lily"

var b =['jon','coco']

b= a.push (b)

console.log(b)//['jon','coco','lily']
```



<font color='red'>**unshit方法**</font>：向数组开头追加元素

<font color='red'>**padStart**</font> & <font color='red'>**padEnd**</font>  字符串追加字符串

![E2FF0616-EAEF-4F0F-8E60-AA1E4054F461](/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.vUCSn2/E2FF0616-EAEF-4F0F-8E60-AA1E4054F461.png)



<font color='red'>**slice**</font>

数组截取或移除。该方法是对数组进行部分截取，并返回一个数组副本；（PS：是一个浅拷贝，复制后的引用都是指向同一个对象的实例，彼此之间的操作会互相影响）参数start是截取的开始数组索引，end参数等于你要取的最后一个字符的位置值加上1

截取数组中的字符串：

![49721789-9608-403B-9A1E-275400269C20](/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.lA7Isd/49721789-9608-403B-9A1E-275400269C20.png)

截取数组中的对象

![55BDDB2D-61BC-44A3-B8F4-FC142EB856D9](/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.1RtRlw/55BDDB2D-61BC-44A3-B8F4-FC142EB856D9.png)

<font color='red'>**split**</font>截取字符串成为数组 （----字符串=>数组------）

![DF199566-68B8-4931-8B65-023EF79D55D4](/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.cvCTZM/DF199566-68B8-4931-8B65-023EF79D55D4.png)

<font color='red'>**every****与some的使用**</font>

这个地方应用到一些类推的地方。

举个栗子：便利店3，分类页面：如果二级分类下面没有商品，那么不展示二级分类标题。如果一级分类下面的所有的二级分类里面都没有商品，那么隐藏一级分类。如果某一项返回为ture那么都为true；如果全部为true那么才为true

every()与some()方法都是JS中数组的迭代方法。

every()是对数组中每一项运行给定函数，如果该函数对**每一项**返回true,则返回true。

some()是对数组中每一项运行给定函数，如果该函数对**任一项**返回true，则返回true。



<font color='red'>**find**</font>的使用---根据条件查找数组中符合当前查找的项，返回一条对象

![2D5A2ED5-26BF-46DD-8263-9F7DB3147290](/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.51MvL0/2D5A2ED5-26BF-46DD-8263-9F7DB3147290.png)

<font color='red'>**filter**</font>的使用---根据条件查找数组中符合当前查找的项，返回数组

```javascript
var arr = [{id:1,name:'coco'},{id:2,name:'kevin'}]
var a = arr.filter(v=>v.id === 1)
```



<font color='red'>**找出数组中最大的值用Math.max ; 找出数组中最小值用Math.min**</font>

```
// es6写法
var arr = [1,2,3,4,5,8]
var a = Math.max(...arr)
console.log(a) // 8 

//es5写法
var arr = [1,2,3,6,4]
var a = Math.max.apply(null,arr)
console.log(a) // 6

// reduce写法
var arr = ['2','4','9']
var a = arr.reduce(num1,num2)=>{
	return num1 > num2 ? num1:num2
}
console.log(a)//'9'

//数组对象中查找出最大的值
//使用reduce返回一个对象
var arr = [{id:1},{id:2}]
var a = arr.reduce((pre,current)=>{
	return (pre.y > current.y) ? pre : current
})
console.log(a)//{id:2}
```



<font color='red'>**把数组中的某个字段组成一个新的数组**</font>

```javascript
var objArray = [ { name: 'lily', id:1}, { name: 'coco', id:2},{ name: 'kevin', id:3}];
var result = objArray.map(function(a) {return a.name});

```

<font color='red'>**foreach**与**map**的使用</font>

区别：

map，返回新数组，通常适用你要改变数据值，这样的好处是可以复合fiter，reduce等组合便于获取所需内容编写方法。

forEach方法一般是对原有的数组进行操作，没有返回值，forEach适合于你并不打算改变数据的时候，而只是想用数据做一些事情 – 比如获取值调接口或则打印出来

map es6的写法：

```javascript
// 1.有return : const aaa= [1,2,3]
aaa.map((item)=>{
 return item*2
})
```

```javascript
// 2.没有return

const aaa = [1,2,3] aaa.map(item=>item*2)
foreach写法：
const aaa = [1,2,3] aaa.foreach((item)=>{    item*2 })
```

<font color='red'>**往数组中，添加键值对**</font>

```javascript
const arr = [{name:'lily',age:10},{name:'coco',age:15},{name:'kevin',age:30}]
arr.map((item,index)=>{item.key = index return item})
```



<font color='red'>**修改数组中的某一项的值**</font>

```javascript
let aaa = [{name:'lily',age:'20',id:1},
{name:'coco',age:'30',id:2}
]
 
aaa.map(item=>{
 if(item.id == 1){
	item.name='kevin'
 }
})
```

<font color='red'>**数组里面的值插入字段名称**</font>

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.yCx5EX/Image.png" alt="Image" style="zoom:75%;" />



<font color='red'>**只获取数组里面的键名或某个键名下的所有值**</font>

```javascript
const boyMap = [{name:'kevin',age:20},{name:'jon',age:40}]
const key = boyMap.map(item=>item.name)
console.log(key)
// ["kevin", "jon"]

```



<font color='red'>**去除数组中的空数组**</font>

`arr.filter(v=>v)`



<font color='red'>**indexOf的使用**：</font>

如果要检索的字符串值没有出现，则该方法返回 -1

注意：

indexOf获取到的筛选值的当前长度值

indexOf对大小写敏感

![8AEC49C1-5A81-4ADD-9312-37CE6F285AE4](/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.4XGLR3/8AEC49C1-5A81-4ADD-9312-37CE6F285AE4.png)

indexOf对空格敏感，会把空格也计算在内的

![6D9C7921-86AA-4009-96AE-379D210B49B6](/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.52zl2Z/6D9C7921-86AA-4009-96AE-379D210B49B6.png)

<font color='red'>**flat的使用**：</font>

**flat()** 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。可以扁平化嵌套数组。并且去除空项

深层嵌套扁平化方法：`flat(Infinity)`

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.qY1rzG/3B76BA95-6D30-4280-A034-86CAE5DA6799.png" alt="3B76BA95-6D30-4280-A034-86CAE5DA6799" style="zoom:50%;" />

<font color='red'>**多维数组降级操作：**</font>

```javascript
const flattenDeep = (arr) => Array.isArray(arr)
  ? arr.reduce( (a, b) => [...a, ...flattenDeep(b)] , [])
  : [arr]
flattenDeep([1, [[2], [3, [4]], 5]])

```

<font color='red'>**比较2个数组，取交集，并集，差值**</font>

1.数组中含对象：

```javascript
1.取差集
    let a=[{id:1,a:123,b:1234},{id:2,a:123,b:1234}];
    let b=[{id:1,a:123,b:1234},{id:2,a:123,b:1234},{id:3,a:123,b:1234},{id:4,a:123,b:1234}];
    let arr = [...b].filter(x => [...a].every(y => y.id !== x.id));
    console.log('arr',arr);

2.取交集
    let a=[{id:1,a:123,b:1234},{id:2,a:123,b:1234}];
    let b=[{id:1,a:123,b:1234},{id:2,a:123,b:1234},{id:3,a:123,b:1234},{id:4,a:123,b:1234}];
    let arr = [...b].filter(x => [...a].some(y => y.id === x.id));
    console.log('arr',arr)

```

2.数组中只含有字符串

```javascript
1.取差集
    var a = [1,2]
    var b = [1,2,3,4]
    var c  = b.filter(key => !a.includes(key))
    console.log(c)
    //结果：[3,4]    

2.取交集
    var a = [1,2]
    var b = [1,2,3,4]
    var c  = b.filter(key => a.includes(key))
    console.log(c)
    //结果： [1, 2]

```

```javascript
判断b和c数组里的值，是否都是存在于a数组里面
let a = [2, 3, 4, 5, 6, 7, 8, 9, 10];
let b = [2, 3];
let c = [2,1]; 

function includes(arr1: any[], arr2: any[]) {
  return arr2.every(val => arr1.includes(val));
}

console.log(includes(a, b)); //true
console.log(includes(a, c)); //false

```

<font color='red'>**有arr数组：【1，2】,brr数组：[ {id:1,name:’kevin’},{id:2,name:’coco’},{id:3,name:’lily'} ]筛选出b数组的id与a数组相符合的值，组成一个新的数组**</font>

```
let filterArr=brr.filter(item=>{
    return arr.includes(item.id)
})

```

<font color='red'>**将数组分成若干等份，然后指定取某一段的数组**</font>

```javascript
1,将数组array分成长度为subGroupLength的小数组并返回新数组
Const group = (array, subGroupLength)=> {
      let index = 0;
      let newArray = [];
      while(index < array.length) {
          newArray.push(array.slice(index, index += subGroupLength));
      }
      return newArray;
  }
 2,例如：
 let Array = [1,2,3,4,5,6,7,8,9,10,11];
 let current = 3  //将数组按3个为一组分割
 let index = 2   //指定取第二段的数组
 var groupedArray = group(Array, current)
 Let arrblock = groupedArray(index-1)
 得到的groupedArray 数组为：
    groupedArray[[1,2,3],[4,5,6],[7,8,9],[10,11]]
    arrblock = [4,5,6]

```



<font color='red'>**快速生成数组的方法：**</font>

```javascript
第一种：
Const arr = Array.apply(null,{length:20}).map(function(item,index){
    return {name:'xxx',index:(index+1)};
});

//[{name:'coco',id:1}]

第二种
new Array(8).fill('').map( (item, index) => index+1)
//[1,2,3,4,5,6,7,8]

```

<font color='red'>**给定一组数组brr，把arr数组对象的值筛选出来**</font>

```javascript
var arr = [{id:1,name:'coco'},{id:2,name:'kevin'},{id:3,name:'chilrs'}]
var brr = [1,2]
var c = arr.filter(arr => brr.indexOf(arr.id) === -1);
console.log(c)

```

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.qSAhUa/66E8BCC5-F53C-4D6B-8530-A1B939213103.png" alt="66E8BCC5-F53C-4D6B-8530-A1B939213103" style="zoom:50%;" />

<font color='red'>**给定一个对象，根据id把数组里替换成这个对象**</font>

```javascript
var list = [
    { id: 1, name: "a" },
    { id: 2, name: "b" },
    { id: 3, name: "c" }
];
var replacement = { id: 2, name: "b", sex: "female" };
for (let i = 0, len = list.length; i < len; i++) {    
    if (list[i].id === replacement.id) {
        list[i] = replacement;
    }
}
console.log(list);

```

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.JJUm3v/594FBD80-2C38-4757-97E0-E2BB789433E1.png" alt="594FBD80-2C38-4757-97E0-E2BB789433E1" style="zoom:50%;" />

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.kW7R43/5B8E4840-DAAD-4B7B-8149-641B5459945A.png" alt="5B8E4840-DAAD-4B7B-8149-641B5459945A" style="zoom:33%;" />

```javascript
let a = {
  default1:1,
  default2:1,
  default3:'',
  levename1:'11',
  levename2:'22',
  levename3:'33',
}
console.log(Object.keys(a).reduce((v,k)=>{
  let num =k.match(/(\d+)[^0-9]*$/)[1];
  let str = k.replace(num,'')
    if (!v[(num - 1)]){
    	v[(num - 1)] = {}
    }
  v[(num - 1)][str] = a[k]
  	return v
},[]))

```

