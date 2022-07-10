### 什么是fs文件系统模块？

fs模块是nodejs官方提供用来操作文件的模块，他提供一系列的方法和属性，用来满足用户对文件的操作需求。

```javascript
例如：

fs.readFile()方法，用来读取指定文件中的内容

fs.writeFile()方法，用来向指定的文件中写入内容
```

如果需要在js代码中，使用fs模块来操作文件，则需要使用如下的方式先导入它：（ps:fs不是包，是运行环境中内置的api，所以不用特地去安装）

```
const fs = require('fs')
```

### 读取指定文件中的内容

fs.readFile()的语法格式：

```javascript
fs.readFile(path[,options],callback)
//参数一：必选参数，字符串，表示文件的路径
//参数二：可选参数，表示以什么编码格式来读取
//参数三：必选参数，文件读取完成后，通过回调函数拿到读取的结果
```

fs.readFile()示例代码：

以utf8编码格式，读取指定文件的内容，并打印err和dataStr的值：

```javascript
const fs=require('fs')
fs.readFile('./text.txt','utf-8',(err,res)=>{
	console.log(err)
	console.log('-----')
	console.log(res)
})
```



### 向指定文件中写入内容

```javascript
fs.writeFile(file,data[,option],callback)
//参数一：必选参数，字符串，表示文件的路径
//参数二：必选参数，表示要写入的内容
//参数三：可选参数，表示要以什么格式写入文件中，默认值为utf8

 //写入内容到指定文件
 const fs=require('fs')
fs.writeFile('./text.txt','baby',(err,res)=>{
    console.log(err)
	console.log('-----')
	console.log(res)
})
```



### __dirname

表示当前文件所处的根目录，无论切换到何处，都能正常读取到文件地址,实现动态读取文件路径.几遍切换到其他层面的文件都能争取读取到文件

```javascript
// 使用文件全部路径
fs.readFile(__dirname+'/text.txt','utf-8',(err,res)=>{
	console.log(err)
	console.log('---coco--')
	console.log(res)
})

console.log(__dirname,'www ') 

//后期不要用加好啦，以后要用path.join()来读取文件啦
const path = require('path')
// 使用path.join的方式读取
fs.readFile(path.join(__dirname,'/myfile/index.txt'),'utf-8',(err,res)=>{
	console.log(res,'xx res xxx')
})
```



### 什么是path路径模块

path模块是nodejs官方提供的，用来处理路径的模块，它提供了一系列的方法和属性，用来满足用户对路径的处理需求

例如：path.join()方法，用来将多个路径片段拼接成一个完整路径字符串

```javascript
//例子3
const path = require('path');
const pathStr = path.join('/a','/b/c','../','./d','e')
console.log(pathStr) ///a/b/d/e

/**
 * 解释：
 * ../可以会抵消紧挨着前面的路径抵消掉，所以上面的例子的c就是../给抵消了，注意./是没有抵消的
 * ..**/

// 例子2
const newPathStr = path.join('/a','/b/c','../../','./d','e')
console.log(newPathStr)
//最终./d是并没有比抵消的哦，而../../则可以抵消2层路径
```

例如：path。basename()方法，用来从路径字符串中，将文件名解析出来,最常见的就是通过这个方法获取路径中的文件名

```javascript
const path = require('path')

// 定义文件的存放路径
const fpath = '/a/b/c/index.html'

const fullName = path.basename(fpath)
console.log(fullName,'--- fullName ----')//index.html

// 只保留文件的文件名，移除文件扩展名
const nameWithowExt = path.basename(fpath,'.html')
console.log(nameWithowExt,'--- nameWithowExt ----')//index

```

例如：path.extname 获取路径中的文件扩展名

```javascript
//仅获取文件扩展名
const extFpath = '/a/b/c/index.js'
const extension = path.extname(extFpath)
console.log(extension,'-- extension --')//.js
```



注意事项：

1.fs.writeFile()方法只能用来创建文件，不能用来创建文件夹

 2.重复调用fs.writeFile()写入统一文件时，新写入的内容会覆盖之前旧内容

