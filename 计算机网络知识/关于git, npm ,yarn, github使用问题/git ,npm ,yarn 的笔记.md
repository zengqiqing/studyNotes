## git ，npm , yarn , github 使用

dependencies 是生产环境下的依赖，项目刚需的依赖在这里，比如UI框架，字体文件等线上必需的东西

下载安装包很慢怎么办？

终端执行全局安装nrm: npm install install nrm -g

查看nrm内置的源：nrm ls

选择其中一种源，例如淘宝：nrm use taobao

然后再执行npm install xxx 那么就可以很快安装完了

---

Git代码回滚方法：https://blog.csdn.net/jiadajing267/article/details/88386132

---

**记录一个坑爹的操作，从此必须记得，操作分支必须要备份分支！！！！！**

我想回退分支，后来瞎搞，是回退了，还推送到远程了，结果所有的分支代码没了，还没有commit记录。也没有版本号了。糟糕，我回退就是为了对分支再操作点东西啊！这下子怎么办！

操作：

`git reflog`

查看具体文件的所有的commit操作命令：

```
git log --stat --full-history --simplify-merges -- src/aliance/views/marketManagement/coupon/couponDetail/activity.vue
```

