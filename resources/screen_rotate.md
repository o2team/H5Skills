# 判断用户手机旋屏

判断用户屏幕大概有这几种方法：

1、CSS Media Queries

**内联样式**

```css
@media screen and (orientation:portrait) {
    //竖屏
}
@media screen and (orientation:landscape) {
    //横屏
}
```

**外联样式**

```html
<!-- 竖屏 -->
<link rel="stylesheet" media="all and (orientation:portrait)" href="..." />
<!-- 横屏 -->
<link rel="stylesheet" media="all and (orientation:landscape)" href="..." />
```

2、window.matchMedia()

```javascript
var mql = window.matchMedia("(orientation: portrait)");
function onMatchMeidaChange(mql){
    if(mql.matches) {
        // 竖屏
    }else {
        // 横屏
    }
}
onMatchMeidaChange(mql);
mql.addListener(onMatchMeidaChange);
```

3、window.innerHeight/window.innerWidth

```javascript
function detectOrient(){
    if(window.innerHeight >= window.innerWidth) {
        // 竖屏
    }else {
        // 横屏
    }
}
detectOrient();
window.addEventListener('resize',detectOrient);
```

4、window.orientation

```javascript
switch(window.orientation) {
    case 0:
        displayStr += "Portrait";
        break;
    case -90:
        displayStr += "Landscape (right, screen turned clockwise)";
        break;
    case 90:
        displayStr += "Landscape (left, screen turned counterclockwise)";
        break;
    case 180:
        displayStr += "Portrait (upside-down portrait)";
        break;
}
```

```javascript
function detectOrient(){
    if (Math.abs(window.orientation) === 90) {
        // 横屏
    } else {
        // 竖屏
    }
}
detectOrient();
window.addEventListener('orientationchange',detectOrient);
```

更详细的内容请移步至：[探讨判断横竖屏的最佳实现](https://aotu.io/notes/2017/01/31/detect-orientation/)
