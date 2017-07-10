# 实现渐变文字与镂空文字

```html
<p class="gradient_txt">渐变文字渐变文字渐变文字渐变文字</p>
```

```css
.gradient_txt {
    background-color: #890ffa;
    background-image: gradient(linear, left top, right top, from(#890ffa), to(#d80078));
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}
```

```html
<p class="hollow_txt">镂空文字镂空文字镂空文字镂空文字</p>
```

```css
.hollow_txt {
    font-size: 50px;
    -webkit-text-stroke: 1px #000;
    -webkit-text-fill-color: transparent;
}
```
