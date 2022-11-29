## css3实现一个滚动轮播的播报器

```html
{* Html文件 *}

<div class="container">
  <ul class="ul" height = "160px">
    <li>kevin</li>
    <li>coco</li>
    <li>yuki</li>
  </ul>
</div>

```



```css

{* css文件 *}

@keyframes scroll{
  0%{
    transform: translate(0,0)
  }
  100%{
    transform: translate(0,-160px)
  }
}

.container{
  @include middle;
  width: 220px;
  height: 40px;
  background: #0066ff;
  border-radius: 2em;
  overflow: hideden;
  .ul{
    animation-name: scroll;
    animation-duration: 5s;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
    // animation: scroll 5s linear infinite; 动画属性简写
  }
  li{
    width: 100%;
    line-height: 40px;
    verticle-align: bottom;
    color: #fff;
    text-align: center;
  }
}

```

