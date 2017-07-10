# 使用 not 简化样式

常规我们写一个导航，每个导航间都有空隙（除了最后一个），以下：

```html
<div class="nav">
    <a href="#">首页</a>
    <a href="#">购物圈</a>
    <a href="#">个人中心</a>
</div>
```

```css
.nav a {
    margin-right: 10px;
}
.nav a:last-child {
    margin-right: 0;
}
```

这一点都不优雅！使用 `not` 来简化样式

```css
.nav a:not(last-child) { /* 除了最后一个 */
    margin-right: 10px;
}
```
