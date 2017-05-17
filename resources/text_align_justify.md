# 文字两端且均分对齐技巧

如下图几种场景，实现英文字母与中文字母两端对齐，且英文字符需要做到均分：

<img src="http://storage.360buyimg.com/mtd/home/b1494990046573.png" width="600">

可以通过两种方案实现：

### 方案一：

通过 flex 布局的 `justify-content: space-between;`属性实现两端对齐，项目之间的间隔都相等。

```html
<div class="jd">
    <i>J</i>D<i>G</i><i>W</i>
</div>
```

```css
. jd {
    display: flex;
}
. jd i {
    width: 1em;
    justify-content: space-between;
}
```

### 方案二（黑科技）：

需要均分的字符间需要有一个空格进行字符之间的分离，结合 `text-align: justify;` 属性以及伪元素。

```html
<div class="jd">J D G W</div>
```

``` css
.jd {
    display: block;
    text-align: justify;
}
.jd::after {
    content: '';
    display: inline-block;
    width: 100%;
}
```
