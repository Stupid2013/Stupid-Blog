## 乱七八糟的vertical-align

2018-02-28

emmm...写这个的初衷是：经常用vertical-align: middle都不起作用的样子，于是，就想研究一下😊

    vertical-align具有的属性：长度，百分值(%)，baseline(默认值)，sub，super，top，text-top，middle，bottom，text-bottom，inherit。

### 一、长度
```html
<div class="wrapper">
  <div class="test">
    <input type="radio" name="test1" value="n" />昨天是晴天
  </div>
</div>
```
```css
.wrapper {
  margin: auto;
  width: 200px;
  height: 120px;
  background: pink;
  border: 1px solid #333;
}
.test {
  background: purple;
  color: white;
}

.test input {
  vertical-align: 1px; // 相对于基线向上偏移一像素
}
```
![长度](images/pic1.png)

参考文章：

  [我对CSS vertical-align的一些理解与认识（一）](http://www.zhangxinxu.com/wordpress/2010/05/%E6%88%91%E5%AF%B9css-vertical-align%E7%9A%84%E4%B8%80%E4%BA%9B%E7%90%86%E8%A7%A3%E4%B8%8E%E8%AE%A4%E8%AF%86%EF%BC%88%E4%B8%80%EF%BC%89/)
