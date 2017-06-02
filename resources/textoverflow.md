# 文字超出省略号

**单行文字超出省略号**

```css
.example {
    width: 200px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-scaple: nowrap;
}
```

**多行文字超出省略号**

```css
.example {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2; /* 指定超过2行省略号 */
    text-overflow: ellipsis;
    overflow: hidden;
}
```
