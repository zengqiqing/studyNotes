1.rn 加载脚本的机制：
在 rn打包时，rn 会将所有的业务 js 文件打包在一个叫：`index.android.bundle/index.ios.bundle`的文件中。所以启动 app 时会第一时间加载 bundle 文件，那么脚本热更新要做的事情就是替换掉这个 bundle 文件。

![image-20210809092133666](/Users/other/Library/Application Support/typora-user-images/image-20210809092133666.png)

2.生成 bundle文件：

