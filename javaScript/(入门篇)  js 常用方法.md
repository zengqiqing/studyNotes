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

