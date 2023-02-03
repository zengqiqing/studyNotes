### chrome 如何分析页面加载时间：

参考资料：

https://blog.csdn.net/qq_42415326/article/details/124456849

https://www.cnblogs.com/amyzhu/p/13125463.html

![描述](http://rls62zajo.hn-bkt.clouddn.com/%E8%B0%B7%E6%AD%8C%E6%8E%A7%E5%88%B6%E5%8F%B0%E5%88%86%E6%9E%90.png)

Finish：页面最后一个请求截止时间，如果页面加载完成，触发请求，那么时间会变更。

DOMContentLoaded：dom内容加载并解析完成时间，即首屏（白屏）加载时间

Load:页面所有资源（图片，音视频，数据等）加载完成时间

![描述](http://rls62zajo.hn-bkt.clouddn.com/%E6%B5%8F%E8%A7%88%E5%99%A81.png)

Overview：查看每帧请求的内容

Timing选项卡：

- Queuing:队列；在 HTTP 1.0/1.1 连接上，Chrome 会将每个主机强制设置为最多六个 TCP 连接。如果一次请求十二个条目，前六个将开始，而后六个将被加入队列。最初的一半完成后，队列中的第一个条目将开始其请求流程

- Stalled 停滞
- DNS lookup DNS查找
- initial connection 初始连接
- SSL handshake SSL握手
- Request sent 请求发送
- Waiting 等待，具体指到开始下载第一个字节的时间（TTFB：time for first byte）
- Content Download 内容下载

![描述](http://rls62zajo.hn-bkt.clouddn.com/timing2.png)

![描述](http://rls62zajo.hn-bkt.clouddn.com/timing3.png)

### 如何优化网络请求问题

🌺.采用域名分片：存在队头阻塞问题，我们可以采用域名分片解决，域名分片通俗解释：多个域名加载网络资源，每个域名再分配多个长链接，最后这些域名都映射到同一个网站服务器，这个可以突破浏览器限制，让链接数增加，域名可以解析成别名（cname），用于cnd附在均衡

- 注释：队头阻塞
  http传输基于“请求-应答”模式，报文必须是一发一收，所有任务被放到一个任务队列中串行执行。当http开启长连接时，当前域名下会共用一个tcp连接，一旦队首因某些原因卡住，后续只能处于pendding状态，这就是“队头阻塞”

- 长链接，短连接的应用

  http1.0默认使用短连接，客户端和服务器每次进行http操作就建立一次连接，任务结束就终止连接。当客户端访问某个html或其他类型web页面中包含其他web资源（js文件，图片，css等）,每遇到一个web资源，浏览器就重新建立一个http会话；从http1.1后默认使用长连接（connection:keep-alive），当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，客户端再次访问这个服务器时，会继续使用这一条已经建立的连接。Keep-Alive不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间。实现长连接需要客户端和服务端都支持长连接。

  适用场景：

  短连接：适用于网页浏览等数据刷新频度较低的场景。

  长连接：适用于客户端和服务端通信频繁的场景，例如聊天室，实时游戏等。
  
  ![描述](http://rls62zajo.hn-bkt.clouddn.com/tcp.png)
  
  
  
  

🌺：使用网站缓存技术，减少服务器处理时间

🌺：使用cdn加速，降低网络拥塞，提高用户访问响应速度和命中率

🌺：减少请求头带上多余的cookie数据信息

🌺：前端图片压缩，减少图片体积，加快图片渲染；去除冗余代码，减少前端项目包体积



