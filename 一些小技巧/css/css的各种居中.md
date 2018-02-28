## css的各种居中

作为一名前端coder，各种各样各种情况下的居中效果可谓是数不胜数，而要快速、准确、优雅、高效的完成这样的css效果可以说是一名前端coder的必备技能了！像我这种小菜鸟，哪怕是曾经做过的效果，如果没有经常使用的话，遇到类似问题时，也不能快速的解决问题。emmm...所以呢，为了让自己慢慢想大牛们靠近，趁着有时间，赶紧充充电，记录一下，哈哈。

### 图片在div中垂直居中

方法一：display：table；

```html
<div id="box">
  <span>
    <img src="./images/pic.jpg" alt="" />
  </span>
</div>
```
```css
#box{
  width:500px;
  height:400px;
  display:table;
  text-align:center;
  border:1px solid #d3d3d3;
  background:#fff;
}
#box span{
  display:table-cell;
  vertical-align:middle;
}
#box img{
  border:1px solid #ccc;
}
<!--[if lte IE 7]>
  <style type="text/css">
  #box{
      position:relative;
      overflow:hidden;
  }
  #box span{
      position:absolute;
      left:50%;top:50%;
  }
  #box img{
      position:relative;
      left:-50%;top:-50%;
  }
  </style>
<![endif]-->
```

效果如图：
![pic1](./images/pic1)


参考文章：

  [未知高度的图片在div设置垂直居中](http://www.cnblogs.com/leejersey/p/3780415.html)
