## git ，npm , yarn , github 使用

dependencies 是生产环境下的依赖，项目刚需的依赖在这里，比如UI框架，字体文件等线上必需的东西

下载安装包很慢怎么办？

终端执行全局安装nrm: npm install install nrm -g

查看nrm内置的源：nrm ls

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.PnKed8/196AC907-3C95-4780-9E79-7CF5EE98D96B.png" alt="196AC907-3C95-4780-9E79-7CF5EE98D96B" style="zoom:33%;" />

选择其中一种源，例如淘宝：nrm use taobao

然后再执行npm install xxx 那么就可以很快安装完了

---

Git代码回滚方法：https://blog.csdn.net/jiadajing267/article/details/88386132

---

**记录一个坑爹的操作，从此必须记得，操作分支必须要备份分支！！！！！**

我想回退分支，后来瞎搞，是回退了，还推送到远程了，结果所有的分支代码没了，还没有commit记录。也没有版本号了。糟糕，我回退就是为了对分支再操作点东西啊！这下子怎么办！

操作：

`git reflog`

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.wBWl7R/328F74CE-7460-4AA0-B269-389C9D6A8306.png" alt="328F74CE-7460-4AA0-B269-389C9D6A8306" style="zoom:33%;" />

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.QG4QfN/ED15A993-0083-4839-95A5-E12D8FD3F178.png" alt="ED15A993-0083-4839-95A5-E12D8FD3F178" style="zoom:50%;" />