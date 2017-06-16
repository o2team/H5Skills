# createjs.Tween.removeTweens 的用法

createjs.Tween.removeTweens 的语法是：

```javascript
createjs.Tween.removeTweens(target);
```

以下是官网解释: 

>Removes all existing tweens for a target. This is called automatically by new tweens if the override property is true.  
>Parameters:  
>  
>target Object  
>The target object to remove existing tweens from.  

target 其实是 DisplayObject 而不是 tween。

```javascript
// 错误的用法
var tween = createjs.Tween.get(instance).to({xx: xxx}, 1000);
createjs.Tween.removeTweens(tween);

// 正确的用法 
var tween = createjs.Tween.get(instance).to({xx: xxx}, 1000);
createjs.Tween.removeTweens(instance); // 使用实例而不是tween
```
