包是基于nodejs内置模块封装出来的，提供了更高级，更方便的api，极大地提高了开发效率。包和内置模块之间的关系，类似于jq与浏览器内置api之间的关系。



包的语义化版本规范

包的版本号以‘点分十进制’形式进行定义的，总共有三位数，例如2.24.0

其中每一位数字所代表的含义如下：

第一位数字：大版本

第二位数字：功能版本

第三位数字：bug修复版本

版本号提升规则，只要前面的版本号增长了，后面的版本号归零

由于第三方包体积可能比较大，所以在开发项目中，一定要把node_modules文件夹添加到.gitgnore忽略文件中，这样上传代码时就不会把node_modules一并上传。



#### 如何快捷创建packjson.json文件

```
//在执行命令所处的目录中，快速新建package.json文件
npm init -y
```

注意：上述命令只能在英文的目录下成功运行！所以项目文件夹名称一定要使用英文命名，不要使用中文，也不要出现空格

安装包可以同时安装多个包，命令如下：

```
//同时安装jquery 和 art-template包
npm install jquery art-template
```



#### 如何卸载包：

```
//使用npm uninstall 具体包名来卸载包
npm uninstall moment
```

注意：npm uninstall 命令执行成功后，会把卸载的包，自动从package.json的dependencies中移除.

#### 卸载全局包：

注意：

1.只有工具性质的包，才有全局安装的必要性，因为他们提供了好用的终端命令

2.判断某个包是否需要全局安装后才能使用，可以（到npm上搜包，然后看看他要不要你装全局）参考官方提供的使用说明即可。

```
npm uninstall 包名 -g
```

#### 包的内部结构

一个规范的包，他的组成结构必须符合以下三点要求：

1.包必须以单独的目录而存在

2.包的顶级目录下要必须包含package.json这个包管理配置文件。

3.package.json中必须包含name,version,main这三个属性，分别代表包的名字，版本号，包的入口

main代表的是这个包的入口文件，查看main只需要在包的package.json中查看就可以知道包配置的入口文件是哪个



#### 开发属于自己的包

1.初始化包结构：

新建文件夹后，随即创建package.json,index.js,READMD.md这三个文件

2.初始化package.json

```js
{
"name":"ithema-tool-coco",//包的真正名称由package.json中的name属性决定，且包的名称不能重复，由于npm有很多包，所以要去npm上检索一下是否与自己的包名称重复。
"version":"1.0.0",//新的包默认从1.0.0版本开始
"main":"index.js",//入口文件
"description":"提供格式化时间，HTMLEscape的功能",
"keywords":["ithema-tool-coco","dateFormat","escape"],
 "license":"ISC",//遵循许可的开源协议，默认协议ISC
}
```

### 发布包的流程

1.注册npm账号

①：访问https://www.npmjs.com/ 网站，点击sign up按钮，进入注册用户界面

②: 填写账号相关的信息：fullName,public Email,Username,Password

③：点击create an account 按钮，注册行号

④：登录邮箱，点击验证链接，进行账号的验证

2.登录npm 

不是说在npm官网上登录，而是需要在终端中执行npm login命令，依次输入用户，密码，邮箱后才算登录成功；注意在运行npm login命令前，必须先把下包的服务器地址切换为npm 官方服务器，否则会发布包失败

 登录时出现到最后一步需要验证码：那么请到邮箱处接收对应的验证码，之后填入即可，例如下面的27904627

```
Enter one-time password from your authenticator app: 27904627
```

3.把包发布到npm上

将终端切换到包的根目录之后，运行npm publish命令，即可将包发布到npm上（注意包名不能雷同 且发布的包名不能大写，会报错的）

4.删除已发布的包

在根目录下运行npm unpublish 包名 --force命令，即可从npm删除已发布的包

注意：

① npm unpublish 命令只能删除72小时内发布的包

② npm unpublish 删除的包，在24小时内不允许重复发布

③ 不要啥没意义的包都丢npm，浪费资源

