# 不定宽块级元素水平居中

在我们的传统方法中，对于这种不定宽要居中的元素，我们一般采取两层的结构，以下：

```html
<div class="more_btn"><span>查看更多</span></div>
```

```css
.more_btn {
	text-align: center;
}
.more_btn span {
	display: inline-block;
	padding: 0 30px;
	height: 40px;
    line-height: 40px;
    background: #4caf50;
    color: #fff;
    font-size: 14px;
    text-align: center;
    border-radius: 4px;
}
```

使用 `width: fit-content` 属性来简化结构

```html
<div class="more_btn">查看更多</div>
```

```css
.more_btn {
	margin: auto;
    padding: 0 30px;
    width: -webkit-fit-content;
    width: fit-content; /* 关键 : 宽度等于内容宽度 */
    height: 40px;
    line-height: 40px;
    background: #4caf50;
    color: #fff;
    font-size: 14px;
    text-align: center;
    border-radius: 4px;
}
```

