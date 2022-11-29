## 日常开发遇到的js技巧

**摘录：**

https://juejin.im/post/6844903856489365518?utm_source=gold_browser_extension#heading-12



#### 检测字符串中是否含有手机号，如果含有手机号则对手机号进行脱敏

```javascript
var regx = /(1[3|4|5|7|8][\d]{9}|0[\d]{2,3}-[\d]{7,8}|400[-]?[\d]{3}[-]?[\d]{4})/g; 
var str = "hello world13255443546你好啊";

var strresult = matchPhoneNum(str,regx);
console.log(strresult);
function matchPhoneNum(str,regx){
    var phoneNums = str.match(regx);
    if(phoneNums){
    for(var i= 0;i<phoneNums.length;i++){
        //手机号全部替换
                //str = str.replace(phoneNums[i],"[*******]");
        var temp  = phoneNums[i]
        //隐藏手机号中间4位(例如:12300102020,隐藏后为132*******)
        temp = temp.replace(/(\d{3})\d{4}(\d{4})/,'$1********');
       // 另一种脱敏写法：隐藏手机号中间4位(例如:12300102020,隐藏后为132****2020)
       // temp = temp.replace(/(\d{3})\d{4}(\d{4})/,'$1****$2');
        str = str.replace(phoneNums[i],temp);
    }
}
    return str;
};
```



### 字符串数据脱敏

```javascript
    /**
     * 字符串数据脱敏
     * @str      内容
     * @beginLen 请传入正整数
     * @endLen   请传入负数或0，🙅‍♂️ 不要传入正整数
     * **/
    static desensitization = (str, beginLen, endLen) => {
        let len = str.length;
        let firstStr = str.substr(0, beginLen);
        let lastStr = str.substr(endLen);
        let middleStr = str
            .substring(beginLen, len - Math.abs(endLen))
            .replace(/[\s\S]/gi, '*');
        let tempStr = Object.is(endLen, 0)
            ? firstStr + middleStr
            : firstStr + middleStr + lastStr;
        return tempStr;
    }
```



### 校验输入框是否输入空格

```javascript
xxx.replace(/(^\s*)|(\s*$)/g, ""); 2. 创建过去七天的数组，如果将代码中的减号换成加号，你将得到未来7天的数组集合
[...Array(7).keys()].map(days => new Date(Date.now() - 86400000 * days));
```



### 生成长度为11的随机字母数字字符串

```javascript
Math.random().toString(36).substring(2);
```

### 生成随机十六进制代码如：'#c618b2'

`'#' + Math.floor(Math.random() * 0xffffff).toString(16).padEnd(6, '0');`



### 60秒信息倒计时： 写在uitls的通用文件上

```javascript
export function resetTime(time){
  var timer=null;
  var t=time;
  var m=0;
  var s=0;
  m=Math.floor(t/60%60);
  m<10&&(m='0'+m);
  s=Math.floor(t%60);
  function countDown(){
    s--;
    s<10&&(s='0'+s);
    if(s.length>=3){
    	s=59;
    m="0"+(Number(m)-1);
  }
  if(m.length>=3){
    m='00';
    s='00';
    clearInterval(timer);
  }
  	console.log(m+"分钟"+s+"秒");
  }
  	timer=setInterval(countDown,1000);
}

```

### 距离活动开始时间(x天xx时xx分xx秒)

```javascript
//计算距离活动开始时间
export function resetTime() {
  //获取当前时间
  var date = new Date();
  var now = date.getTime();
  //设置截止时间
  var str = "2019/6/1 00:00:00";
  var endDate = new Date(str);
  var end = endDate.getTime();
  //时间差
  var leftTime = end - now;
  //定义变量 d,h,m,s保存倒计时的时间
  var d, h, m, s;
  if (leftTime >= 0) {
    d = Math.floor(leftTime / 1000 / 60 / 60 / 24);
    h = Math.floor(leftTime / 1000 / 60 / 60 % 24);
    m = Math.floor(leftTime / 1000 / 60 % 60);
    s = Math.floor(leftTime / 1000 % 60);
  }
    //将倒计时赋值到div中
    console.log('距离活动开始还有'+d+'天'+h+'时'+m+'分'+s+'秒')
    //递归每秒调用countTime方法，显示动态时间效果
    setTimeout(resetTime, 1000);
}
```

### 设置和获取cookie值

![58D99B96-D3EC-43E0-A54A-233E302BBD2F](/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.pftDos/58D99B96-D3EC-43E0-A54A-233E302BBD2F.png)

### 正则校验密码内容（必须含有数字，大小写字母，且字符长度为6位以上）

```javascript
const pattern = /^(?=.*?[a-z])(?=.*?[A-Z])(?=.*?\d)(?=.*?[!#@*&.])[a-zA-Z\d!#@*&.]{8,}$/,
const str = '1#2s23sdfa4qqQ';

console.log(pattern.test(str)); //ture
```

