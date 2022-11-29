### Es5语法温故



---------------------------- DOM内置对象 ----------------------------- 

仅记录本人不太熟悉的属性

##### document.write：直接写入 HTML 输出流

- document.write这种方式加载的js是异步的

- 在载入页面后，浏览器输出流自动关闭。在此之后，任何一个对当前页面进行操作的document.write()方法将打开—个新的输出流，它将清除当前页面内容(包括源文档的任何变量或值)

  

###### innerHTML ：设置或返回表格行的开始和结束标签之间的 HTML

- 获取value值，其实我觉得他有点像e.target.value或者是双向绑定获取输入的值一样。先获取元素，然后再通过innerHTML的方式获取或展示输入的值

  使用语法：

  ```javascript
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>es5语法</title>
    </head>
    <style>
      #box {
        width: 250px;
        height: 300px;
        border: 1px solid #e5e5e5;
        background: #f1f1f1;
      }
    </style>
    <body>
      <div id="box"></div>
      <span id="span1">张三：</span>
      <input type="text" id="text1" />
      <input id="btn" type="button" value="发送" name=""/>
      <script>
        window.onload = function(){
            var oBox = document.getElementById("box");
            var oSpan = document.getElementById("span1");
            var oText = document.getElementById('text1');
            var oBtn = document.getElementById('btn');
            oBtn.onclick = function(){
              oBox.innerHTML += oSpan.innerHTML + oText.value + "<br/>"
            }
        }
      </script>
    </body>
  </html>
  
  ```

##### createElement 创建元素节点

该方法可返回一个Element对象

可以结合appendChild属性使用，往a元素后面创建新的b元素

```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>es5语法</title>
  </head>
  <body></body>
</html>
<script language="javascript">
  //写法一：创建元素
  var o = document.body;
  //创建链接
  function createA(url, text) {
    var a = document.createElement("a");
    a.href = url;
    a.innerHTML = text;
    a.style.color = "red";
    o.appendChild(a);
  }
  createA("www.baidu.com", "百度");
//写法二：结合appendChild使用
  var board = document.getElementById("board");
    var e = document.createElement("input");
    e.type = "button";
    e.value="按钮"
    var object = board.appendChild(e);
</script>

```



##### setattribute 给新增的元素添加属性

```javascript
 <body>
    <div id="board">啦啦啦</div>
  </body>

<script language="javascript">
  function test() {
    console.log("hello");
  }
  var board = document.getElementById("board");
  var e2 = document.createElement("input");
  /* 使用setAttribute */
  e2.setAttribute("type","text");
  e2.setAttribute("name","q");
  e2.setAttribute("value","使用setAttribute");
  e2.setAttribute('onclick',"javascript:alert('hello!!!');");
  var object = board.appendChild(e2)

  /* 不使用setAttribute */
  // e2.type = "text";
  // e2.value="使用setAttribute";
  // e2.onclick = test
  // var object = board.appendChild(e2)
</script>
```



##### location

- loaction 对象包含当前加载页面的URL信息

  ```
  |   属性  |  内容  |
  |  ----  | ----  |
  | location.href  | "http://www.baidu.com:8080/tools/goodsdetail?id=3453#list" |
  | location.protocol  | http: |
  | location.host  | www.baidu.com:8080 |
  | location.hostname  | www.baidu.com |
  | location.port  | 8080 |
  | location.pathname  | /tools/goodsdetail |
  | location.search  | ?id=3453 |
  | location.hash  |  #list |
  ```

##### 跳转，导航

Location.href:直接设置对象href属性，导航到新页面

Location.replace: 用新的url替换旧的url

location.reload : 重新加载当前页面，刷新当前页面（如果使用没有参数的reload( )方法，当浏览器的缓存里保存了当前页面时，就会加载缓存的内容，为了避免这种情况的发生，确保从服务器获得页面数据，可以在调用reload( )方法是添加参数true）----document.reload(true)

History对象有三个方法：

History.forward：点击浏览器的“前进”

History.backward：点击浏览器的“后退”

History.go(2) : 前进2个页面

History.go(-3) : 后退3个页面

提问：

script标签放的位置不同有什么不一样的作用的？