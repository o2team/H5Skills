# input 标签相关样式

**修改 iOS 下 input disabled 颜色**

```css
input:disabled {
    -webkit-text-fill-color: #ddd;
}
```

**设置 input 标签 placeholder 属性的样式**

```css
.example::-webkit-input-placeholder {
    color: red;
}
```

**取消 input 默认样式**

```css
input {
    border: none;
    background: none;
    appearance: none;
    border-radius: 0;
}
```

**取消点击 a、button、input 标签后区域半透明遮罩**

```css
a, button, input {
    -webkit-tap-highlight-color: rgba(255, 0, 0, 0);
}
```

**关闭 iOS 默认输入法首字母大写**

```html
<input type="text" autocapitalize="off">
```

