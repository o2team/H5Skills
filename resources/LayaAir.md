# LayaAir 游戏引擎初体验踩坑指南

总结在使用 LayaAir 游戏引擎制作 H5 游戏中遇到的一些问题。

**1、使用 HTMLDivElement 文本绘制**

针对某一段文字中，某个数字需要加粗或换颜色的情况，可以使用 HTML 文本，默认情况下 htmlDivElement 的宽度为 200，文字到一定长度会自动换行，可以通过 `p.style.wordWrap = false;` 强制不换行或者将 `width` 的值设置为更大解决。

> 引入 laya.html.min.js


```javascript
var p = new HTMLDivElement();
Laya.stage.addChild(p);
p.style.font = "-apple-system, Helvetica, sans-serif";
p.style.fontSize = 30;
p.style.wordWrap = false; // 强制不换行
p.width = 600; // 将默认 200 的宽度改为 600
p.innerHTML = "命中率" + "<span style='color: #f00'>" + 50% + "</span>" // 通过这种方式改颜色
```

**2、绝对定位于底部**

如果只需做竖屏或者横屏的页面时，我们只需要将 `Laya.stage.height` 舞台的高度减去元素高度即可，但是如果同时需要兼容横竖屏，导致高度不一，这样定位就稍稍有些不方便了，这时可以使用 UI 中的 Box 容器，他提供了 `top`、`bottom`、`left`、`right` 等属性，可以方便的定位。

> 引入 laya.ui.min.js

```javascript
var Box = Laya.Box;
var Sprite = Laya.Sprite;

var container = new Box(); // 容器
Laya.stage.addChild(container);
container.bottom = 0; // 基于底部定位
container.left = 0;
container.width = 1334;
container.height = 30;

var sp = new Sprite();
container.addChild(sp); // 将元素添加到容器中
sp.graphics.drawRect(0, 0, 200, 30, "#ffff00");
```

**3、drawPath 实现线条末端线帽的样式**

目前自定义路径的末端样式 `lineCap` 在 WebGL 渲染模式下无效，只能在 Canvas 模式下才行。

```javascript
var sp = new Sprite();
Laya.stage.appChild(sp);
sp.drawPath(50, 50, [
    ["moveTo", 0, 0],
    ["LineTo", 100, 0],
    ["closePath"]
], {
    fillStyle: "#00ffff"
}, {
    lineWidth: 14,
    lineCap: "round" // WebGL 模式下无效
});
```

**4、Matter.js 旋转画布后的鼠标约束**

LayaAir 集成了 Matter.js 物理引擎，Matter.js 的鼠标约束是它自身控制的，Matter.js 的显示是由 LayaAir 控制的，使用 LayaAir 旋转画布方向，但 Matter.js 不会旋转，所以会导致鼠标约束失效。解决方案是使用系统的横屏或者去修改 Matter.js 的源码，如果有约束或鼠标约束的事件，不建议旋转画布。

```javascript
Laya.stage.screenMode = Stage.SCREEN_HORIZONTAL; // 自动横屏旋转画布后会导致 Matter.js 鼠标约束失效
```

**5、使用 LayaAir 缩放后，修正 Matter.js 鼠标约束坐标**

```javascript
Laya.stage.on("resize", this, onResize);

function onResize() {
    // 设置鼠标的坐标缩放
	// Laya.stage.clientScaleX 代表舞台缩放
	// Laya.stage._canvasTransform 代表画布缩放
	Matter.Mouse.setScale(mouseConstraint.mouse, {
		x: 1 / (Laya.stage.clientScaleX * Laya.stage._canvasTransform.a),
		y: 1 / (Laya.stage.clientScaleY * Laya.stage._canvasTransform.d)
	});
}
```

**6、监听动画到某一帧**

监听动画到达某一帧，需要给动画使用 `addLabel` 方法添加标签，再监听动画指定标签即可。

```javascript
var anim = new Animation();
anim.addLabel("now", 2); // 当播放到第二帧时返回 "now"  字符串
anim.play();

anim.on(Event.LABEL, this, function(ev) { // 监听 LABEL 事件
    if (ev == "now") {
        console.log("到达第二帧");
    }
});
```

**7、改变元素层级**

改变元素的层级可以使用 `zOrder` 属性

```javascript
var sp = new Sprite();
Laya.stage.addChild(sp);
sp.zOrder = 10;
```

**8、操作 Laya.init 生成的 Canvas**

```javascript
Laya.init(width, height, Laya.WebGL); // 返回 DOM 元素 Canvas
```

**9、元素点击区域需要加 size 属性**

```javascript
var sp = new Sprite();
Laya.stage.addChild(sp);
sp.graphics.drawRect(10, 166, 166, 90, "#ffff00");
sp.size(166, 90); // size 的大小等于 sp 的宽高，就是鼠标的点击区域

sp.on("MOUSE_DOWN", this, function() {
    console.log("点击触发！");
});
```
