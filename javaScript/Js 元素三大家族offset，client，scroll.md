## 

| client               | 只读                                                         |
| :------------------- | ------------------------------------------------------------ |
| Element.clientWidth  | 元素内部的<font color="red">宽度</font>(单位像素)，包含内边距(padding)，但不包括垂直滚动条、边框(border)和外边距(margin) |
| Element.clientHeight | 元素内部的<font color="red">高度</font>(单位像素)，包含内边距(padding)，但不包括横向滚动条、边框(border)和外边距(margin) |
| MouseEvent.clientX   | 鼠标距离可视区域左侧距离                                     |
| MouseEvent.clientY   | 鼠标距离可视区域上侧距离                                     |
| Element.clientTop    | 表示一个元素顶部边框的宽度                                   |
| Element.clientLeft   | 表示一个元素的左边框的宽度                                   |



| offset               | 只读                                                         |
| -------------------- | ------------------------------------------------------------ |
| Element.offsetHeight | 通常，元素的offsetHeight是一种元素CSS宽度的衡量标准，包含元素的边框(border)、水平线上的内边距(padding)、竖直方向滚动条(scrollbar)（如果存在的话）、以及CSS设置的宽度(width)的值。 |
| Element.offsetWidth  | 通常，元素的offsetWidtht是一种元素CSS宽度度的衡量标准，包含元素的边框(border)、水平线上的内边距(padding)、水平方向滚动条(scrollbar)（如果存在的话）、以及CSS设置的宽度(width)的值。 |
| Element.offsetLeft   | 返回当前元素左上角相对于  Element.offsetParent 节点的左边界偏移的像素值 |
| Element.offsetTop    | 返回当前元素相对于其 Element.offsetParent 元素的顶部内边距的距离。 |
| Element.offsetParent | 返回父系盒子中带有定位的盒子节点，1.返回该对象带有定位的父级 2.如果当前元素的父级元素没有CSS定位， offsetParent为body；如果当前元素的父级元素中有CSS定位，offsetParent 取最近的那个有定位的父级元素。和盒子本身有无定位无关。 |
| Element.offsetX      | 规定了事件对象与目标节点的内填充边（padding edge）在 X 轴方向上的偏移量。(目标节点坐上角为原点) |



| scroll                         | 只读 & 修改                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| Element.scrollWidth(只读)      | 对内容宽度的一种度量，包括由于overflow溢出而在屏幕上不可见的内容，元素内部的高度(单位像素)，包含内边距(padding)，但不包括竖直滚动条、边框(border)和外边距(margin) |
| Element.scrollHeight(只读)     | 对内容高度的一种度量，包括由于overflow溢出而在屏幕上不可见的内容，元素内部的高度(单位像素)，包含内边距(padding)，但不包括水平滚动条、边框(border)和外边距(margin) |
| Element.scrollTop(读取或设置)  | 一个元素的 scrollTop 值是这个元素的内容顶部（卷起来的）到它的视口可见内容（的顶部）的距离的度量。当一个元素的内容没有产生垂直方向的滚动条，那么它的 scrollTop 值为0 |
| Element.scrollLeft(读取或设置) | 一个元素的 scrollLeft 值是这个元素的内容左部（卷起来的）到它的视口可见内容（的左部）的距离的度量。当一个元素的内容没有产生垂直方向的滚动条，那么它的 scrollLeft 值为0 |
|                                |                                                              |

