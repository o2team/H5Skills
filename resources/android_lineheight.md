# 解决 Android 浏览器下 line-height 垂直居中偏离问题

移动端网页开发中，经常会涉及到一些小按钮或者小标签，比如这种：

<img src="http://storage.360buyimg.com/mtd/home/a_021494939266461.png">

对于一般 PC 浏览器以及 iOS 设备的浏览器表现就是我们想要的居中效果，但是大部分 Android 设备的浏览器文字都会稍微向上偏离，如下图所示：

<img src="http://storage.360buyimg.com/mtd/home/b_021494939268926.png">

测试表明，字体字号为奇数的两倍时（比如 10px、14px、18px、22px、26px），会出现严重偏移现象。

其实系统之间效果的差异跟我们的字体类型、系统排版引擎、浏览器都有关系，其实不影响用户浏览与操作等体验，我们可以忽略这些问题，对于一些居于使用场景偏离的比较明显的，可以使用下面提到的两种处理方案。

### 方案一：

我们可以通过 transform: scale 来处理，比如，字体大小是 8px，我们把字体设定为 16px，然后通过 scale(0.5) 缩放至一倍大小，简单粗暴。

注意：放大两倍会使得容器被撑开占位。

### 方案二：

结合行高、对齐的关系，结合伪元素得出的黑科技，亲测效果很理想。

```
className::before {
    content: '';
    display: inline-block;
    vertical-align: middle;
    width: 0;
    height: 100%;
    margin-top: 1px;
}
```