# 移动端技巧集合

## 修改 iOS 下 input disabled 颜色

```css
input:disabled {
    -webkit-text-fill-color: #ddd;
}
```

## 设置 input 标签 placeholder 属性的样式

```css
.example::-webkit-input-placeholder {
    color: red;
}
```

## width: fit-content 不定宽块级元素居中

```html
<div class="more_btn">查看更多</div>
```

```css
.more_btn {
    margin: auto;
    padding: 0 30px;
    width: -webkit-fit-content;
    width: fit-content; /* 关键 */
    height: 40px;
    line-height: 40px;
    background: #4caf50;
    color: #fff;
    font-size: 14px;
    text-align: center;
    border-radius: 4px;
}
```

## 取消表单默认样式

```css
input {
	border: none;
    background: none;
    appearance: none;
    border-radius: 0;
}
```

## 渐变文字与镂空文字

```html
<p class="gradient_txt">渐变文字渐变文字渐变文字渐变文字</p>
<p class="hollow_txt">镂空文字镂空文字镂空文字镂空文字</p>
```

```css
.gradient_txt {
    background-color: #890ffa;
    background-image: gradient(linear, left top, right top, from(#890ffa), to(#d80078));
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}

.hollow_txt {
    font-size: 50px;
    -webkit-text-stroke: 1px #000;
    -webkit-text-fill-color: transparent;
}
```

## not 匹配非指定元素

```css
li:not(:nth-last-child(-n+4)) { /* 除了倒数 4 个 */
	margin-bottom: 10px;
}

li:not(:last-child) { /* 除了最后 1 个 */
	margin-bottom: 10px;
}
```

## iOS Android 全屏视频

iOS 下通过 `webkit-playsinline`、`playsinline` 实现内嵌全屏，不调用系统全屏，有必要的情况下配合 [iphone-inline-video](https://github.com/bfred-it/iphone-inline-video) 库使用。

```html
<video src="video.mp4" webkit-playsinline playsinline></video>
```

```css
/* 隐藏 iOS 视频的播放暂停按钮 */
video::-webkit-media-controls-play-button,
video::-webkit-media-controls-start-playback-button {
    opacity: 0;
    pointer-events: none;
    width: 5px;
}
```

Android 下通过 `x5-video-player-type="h5"`、`x5-video-player-fullscreen="true"` 可视频全屏，同时顶部的微信标题条也会消失，视频内会有一个全屏的 icon 按钮，退出全屏会使视频暂停

```html
<video src="video.mp4" x5-video-player-type="h5" x5-video-player-fullscreen="true"></video>
```

## 判断浏览器是否支持 WebP 图片格式

```javascript
(function() {
    var img = new Image();
    img.onload = function() {
        if (img.width > 0 && img.height > 0) {
            document.documentElement.className = "webp";
            window.webpSupport = true;
        }
    }
    img.onerror = function() {}
    img.src = "data:image/webp;base64,UklGRjoAAABXRUJQVlA4IC4AAACyAgCdASoCAAIALmk0mk0iIiIiIgBoSygABc6WWgAA/veff/0PP8bA//LwYAAA";
}());
```

## 判断浏览器是否支持 APNG

```javascript
(function() {
    "use strict";
    var apngTest = new Image(),
    ctx = document.createElement("canvas").getContext("2d");
    apngTest.onload = function () {
        ctx.drawImage(apngTest, 0, 0);
        self.apng_supported = ctx.getImageData(0, 0, 1, 1).data[3] === 0;
    };
    apngTest.src = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAACGFjVEwAAAABAAAAAcMq2TYAAAANSURBVAiZY2BgYPgPAAEEAQB9ssjfAAAAGmZjVEwAAAAAAAAAAQAAAAEAAAAAAAAAAAD6A+gBAbNU+2sAAAARZmRBVAAAAAEImWNgYGBgAAAABQAB6MzFdgAAAABJRU5ErkJggg==";
}());
```

## 隐藏滚动条

iOS 版本微信从 UI WebView 升级到 WK WebView 后，偶尔会暂时失效

```css
.example::-webkit-scrollbar {
    width: 0;
    height:0;
    display: none;
}
```

## 微信查看当前是 UI WebView 还是 WK WebView

微信进入任意页面，向下拉动页面，通过地址栏来查看

WK WebView 为：此网页由 xxx.xx.com 提供
UI WebView 为：网页由 xxx.xx.com 提供

## 弹性滚动

```css
/* 目前仅支持iOS */
.example {
    -webkit-overflow-scrolling: touch;
}
```

## CSS3 滤镜

```css
.example-1 {
    -webkit-filter: blur(5px); /* 毛玻璃 */
}

.example-2 {
    -webkit-filter: grayscale(0.5); /* 灰度 */
}

.example-3 {
    -webkit-filter: sepia(0.5); /* 褐色 */
}

.example-4 {
    -webkit-filter: brightness(3); /* 亮度 */
}

.example-5 {
    -webkit-filter: hue-rotate(180deg); /* 色相 */
}

.example-6 {
    -webkit-filter: invert(1); /* 反色 */
}

.example-7 {
    -webkit-filter: opacity(0.5); /* 透明度 */
}

.example-8 {
    -webkit-filter: saturate(5); /* 饱和度 */
}

.example-9 {
    -webkit-filter: contrast(0.5); /* 对比度 */
}

.example-10 {
    -webkit-filter: drop-shadow(10px 10px 5px rgba(0, 0, 0, 0.9)); /* 阴影 */
}
```

## 单行文字超出省略号

```css
.example {
    width: 200px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-scaple: nowrap;
}
```

## 多行文字超出省略号

```css
.example {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2; /* 溢出超过2行省略号 */
    text-overflow: ellipsis;
    overflow: hidden;
}
```

## user-select 属性

```css
.example {
    user-select: none; /* 禁止选择 */
    user-select: auto; /* 浏览器来决定是否允许选择 */
    user-select: all; /* 可以选择任何内容 */
    user-select: text; /* 只能选择文本 */
    user-select: contain; /* 选择绑定的元素以内的内容 */
}
```

## 判断用户手机旋屏

推荐文章：[探讨判断横竖屏的最佳实现](https://aotu.io/notes/2017/01/31/detect-orientation/)

## 取消点击 a、button、input 标签后区域半透明遮罩

```css
a, button, input {
    -webkit-tap-highlight-color: rgba(255,0,0,0);
}
```

## 使用 Chrome 模拟手机调试时字号太大

Google Chrome 默认浏览器字体最小字体为：12px，而我们手机端页面常常字体小于 12px

解决：右上角（自定义及控制） -> 设置 -> 显示高级设置 -> 网络内容（自定义字体） -> 最小字号（最小可以设置为 6px）

## 监听 CSS3 动画事件

```javascript
/*
动画开始事件：webkitAnimationStart
动画结束事件：webkitAnimationEnd
动画循环事件：webkitAnimationIteration
过渡动画结束事件：webkitTransitionEnd
 */
example.addEventListener("webkitAnimationStart", function() {
    console.log("动画开始"); // 动画开始执行一次
}, false);

example.addEventListener("webkitAnimationIteration", function() {
    console.log("动画循环结束"); // 动画每循环完成一次执行一次
}, false);
```

## 判断用户是否在手机端

```javascript
if (/Android|Windows Phone|webOS|iPhone|iPod|BlackBerry/i.test(navigator.userAgent)) {
    window.location.href = "http://www.example.com";
}
```

## 图片水平镜像反转

```css
.example-1 {
    transform: scaleX(-1); /* 方法一 */
}
.example-2 {
    transform: rotateY(180deg); /* 方法二 */
}
```

## 自旋转动画

```html
<div class="example"></div>
```

```css
.example {
    position: absolute;
    top: 200px;
    left: 200px;
    width: 100px;
    height: 100px;
    border: 2px solid #000;
    border-radius: 50%;
    animation: autogyRation 4s linear infinite;
}

@keyframes autogyRation {
    from {
        transform: rotate(0deg) translate(-60px) rotate(0deg);
    }
    to {
        transform: rotate(360deg) translate(-60deg) rotate(-360deg);
    }
}
```

## 穿透属性

当元素叠在一起时，需要触发下层的元素时，给上层元素加该属性

```css
.example {
    pointer-event: none;
}
```

## 使 :active 生效

```html
<body ontouchstart>
```

## 禁用自动识别页面中的电话号码

```html
<meta name="format-detection" content="telephone=no">
```

## 微信 CSS 支持度查询

iOS 系统微信使用的是 Safari 的内核，查支持度到 [Can I use](http://caniuse.com/) 即可

Android 则不同，还得看 X5 的脾气，所以以下是查支持度的步骤：

* 查浏览器 UA

* 查看 UA 中的 TBS 版本号
 * QQ 浏览器：6.2 版本及以后使用 Blink 内核
 * X5 tbs 1.x 版本号为 02xxxx，使用 Webkit 内核
 * X5 tbs 2.x 版本号为 03xxxx。使用 Blink 内核
 * X5 tbs 3.x 版本号为 04xxxx。使用 Blink 内核
 * UA 包含 `TBS/0365xx` 字段即说明当前环境已升级到 TBS 2.1 版本
* 通过 [X5 Can I use](http://res.imtt.qq.com/tbs/incoming20160607/home.html) 查属性的支持度
