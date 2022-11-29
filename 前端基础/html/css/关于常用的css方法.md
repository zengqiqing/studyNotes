## 常用的css方法：

### 🌺 常用的flex系列：

#### 列表排列

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.kyIfvB/Image.png" alt="Image" style="zoom: 33%;" />

```html
<div class="box box-tall box-16">
  <div class="box-item">1</div>
  <div class="box-item">2</div>
  <div class="box-item">3</div>
  <div class="box-item">4</div>
  <div class="box-item">5</div>
  <div class="box-item">6</div>
  <div class="box-item">7</div>
  <div class="box-item">8</div>
  <div class="box-item">9</div>
</div>

<style>
  .box-item {
    width: 180px;
    height: 180px;
    line-height: 180px;
    vertical-align: middle;
    margin: 9px;
    background-color: #ffd200;
    font-size: 100px;
    color: white;
    text-align: center;
   }
   .box-16 {
    flex-wrap: wrap;
    align-content: flex-start;
   }
   .box-tall {
    height: 800px;
   }
   .box {
    background-color: white;
    margin: 0 0 55px;
    display: flex;
   }
</style>

```



🌺：