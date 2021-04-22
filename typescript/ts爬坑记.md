- 有a.ts和b.ts两个文件，在a.ts文件中定义一个值`const foo = 123`，当在b.ts文件中同样定义`const foo = 123`会编译报错，报错信息：`Cannot redeclare block-scoped variable 'foo'.`为什么呢？

  ```typescript
  //问题原因：如果一个.ts文件没有包含import或者export,那么他被视为脚本，该文件中所有内容都被视为全局内容，其他文件可以直接使用，反之若使用了import或export那么该文件会被视为一个module，里面的内容只有被别的文件import了才能使用。所以出现重名时就会提示就会提示以上错误。
  
  //解决问题方案：
  /**
  	1.在文件中添加import或export，将文件变为一个模块
  	2.tsconfig.json配置一下，（但我不生效，所以采用第一个方法，后面再跟进一下这个问题。）
  	{
      "compilerOptions": {
          "lib": [
              "es2015"
          ]
      }
  }
  **/
  ```

  