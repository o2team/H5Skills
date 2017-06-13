# setTimeout and setInterval for createjs

## 原生API带来的问题

`createjs` 并没有提供替代 `setTimeout` 和 `setInterval` 的API。如果使用原生的 API 会造成一些意想不到的麻烦，例如我想暂停游戏和游戏的倒计时（setInterval）需要分成两部分来处理：

1. createjs.Ticker.paused = 1; 
2. clearInterval

看上去好像还好对吧。如果要从暂停恢复到继续状态就麻烦了： 

1. createjs.Ticker.paused = 0; 
2. 重新写一个倒计时 setInteval

尽管可以实现，但是代码量比较大，二是会显得零散。

## 解决方案

笔者目前是借助 `tweenjs` 来重写 setTimeout 和 setInterval。如下： 

```javascript
// 定制一个 setTimeout 方法
createjs.setTimeout = function(cb, delay) {
    var timeout = new createjs.Container(); 
    createjs.Tween.get(timeout)
        .wait(delay)
        .call(function() {
            cb && cb(); 
            createjs.clearTimeout(timeout); 
        }); 
    return timeout; 
}
// 定制一个 clearTimeout 方法
createjs.clearTimeout = function(timeout) {
    // 删除动画
    createjs.Tween.removeTweens(timeout); 
    // 删除实例
    timeout = undefined; 
}
```

```javascript
// 定制一个 setInterval 方法
createjs.setInterval = function(cb, delay) {
    var interval = new createjs.Container(); 
    createjs.Tween.get(interval)
        .wait(delay)
        .call(function() {
            cb && cb(); 
        })
        .loop = 1; // 无限循环 
    return interval; 
}
// 定制一个 clearInterval 方法
createjs.clearInterval = function(interval) {
    // 删除动画
    createjs.Tween.removeTweens(interval); 
    // 删除实例
    interval = undefined; 
}
```
使用新写的 `setTimeout` 和 `setInterval` 可以把暂停和恢复游戏变得很简单： 
- createjs.Ticker.paused = 1; // 暂停游戏和游戏倒计时
- createjs.Ticker.paused = 0; // 恢复游戏和游戏倒计时

