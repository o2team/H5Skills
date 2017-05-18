# 通过 :empty 选择器区分样式

> :empty 选择器匹配没有子元素（包括文本节点）的每个元素。

这里举个小红点的例子：

<img src="http://storage.360buyimg.com/mtd/home/a1495073107239.png" width="400">

如图所示，小红点有内容以及无内容的样式差异，按照常规的处理方式，我们一般是通过类名区分，但是我们可以简单通过`:empty`选择器区分开。

```html
<div class="jd"><i>3</i></div>
```

```css
.jd i:empty {
    // 无内容的小红点样式
}
.jd i:not(:empty) {
    // 有内容的小红点样式
}
```