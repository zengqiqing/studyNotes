**Antd 去除表单中的input框里输入的值的前后空白：**

重点代码：`getValueFromEvent: event => event.target.value.replace(/\s+/g, "")`

```javascript
<Item label="第三方商品编号">
  {getFieldDecorator("upid", {
  initialValue: "",
  getValueFromEvent: event => event.target.value.replace(/\s+/g, "")
  })(
  <Input
    placeholder="第三方商品编号"
    disabled={suningSearchShow && !form.getFieldValue("upid")}
	/>
)}
</Item>

```



**Antd table去掉全选按钮**

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.51dPgT/EDB390CC-C17B-4252-97D1-A6292DBFA1CB.png" alt="EDB390CC-C17B-4252-97D1-A6292DBFA1CB" style="zoom: 33%;" />



**Antd table 设置为单选状态，并且不能全选：**

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.j4UvYZ/5E5B4F60-73AA-4EC4-AC59-644F2BE749A9.png" alt="5E5B4F60-73AA-4EC4-AC59-644F2BE749A9" style="zoom: 50%;" />

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.YCFgjx/EC9F0592-B5CD-49B5-B459-8292DD393723.png" alt="EC9F0592-B5CD-49B5-B459-8292DD393723" style="zoom:50%;" />

**Antd 模糊搜索 select組件，如果不想把選擇的結果回填到input裡面那麼可以這樣設置：**

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.TCcdSp/52FA029B-6B26-4AB1-B1B0-D12E58753CF9.png" alt="52FA029B-6B26-4AB1-B1B0-D12E58753CF9" style="zoom:50%;" />

**Antd Select 搜索组件，placeholder不生效的：**

不生效的原因是因为设置了value值，如果有的时候不得不设置value时，那么当value没有值的时候value设为undefined，placeholder就可以生效了

*React 组件写法导致useState时，把整个父组件都重新渲染了。*

![9AD3BBC0-A8D8-4970-AA15-E5F72D418CDD](/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.yZlbfE/9AD3BBC0-A8D8-4970-AA15-E5F72D418CDD.png)

**阻止浏览器自动填充input框：**

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.tvUFNq/8E1ED527-9AC9-451C-864E-A413C6F28B53.png" alt="8E1ED527-9AC9-451C-864E-A413C6F28B53" style="zoom:50%;" />

**进入页面，滚动条要置底部（注意依赖，因为要要有数据才可能会有滚动条，有滚动条才有eleLeft.scrollHeight的赋值）**

```jsx
const [data, setData] = useState(null)
const hei = window.innerHeight;
  useEffect(() => {
    if(data && data.length){
    const eleLeft = document.getElementById("left");
    if(eleLeft.scrollHeight > eleLeft.clientHeight) {
    	eleLeft.scrollTop = eleLeft.scrollHeight
    }
  }
}, [data])
  return (
    <Row type="flex" justify="space-between">
      <Col id='left' span={11} style={{overflow:'scroll', height: hei}}>
     	 <pre dangerouslySetInnerHTML={{ __html: data && data[0] ? data[0].consoleOutputHtml:'' }} />
      </Col>
    </Row>
  );
};

```

**antd 4.0Tree控件问题**

<img src="/var/folders/65/71xg97ds6hnc7_6xq9b987rh0000gp/T/com.yinxiang.Mac/WebKitDnD.DGh9jc/F2B7CE8D-4727-43A9-969F-3A6C17F46651.png" alt="F2B7CE8D-4727-43A9-969F-3A6C17F46651" style="zoom:50%;" />