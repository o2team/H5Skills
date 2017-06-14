# 谈谈 tween 的暂停方法: pause

官网的解释如下： 

>Queues an action to pause the specified tween.  
>Parameters: 
>  
>tween Tween
>The tween to pause. If null, it pauses this tween.  
>Returns:  
>  
>Tween: This tween instance (for chaining calls)

其实用法并没有说清楚。正确的用法是：
```javascript
createjs.Tween.get(instance).pause(tween); 
```

重点在于，除了指定要暂停的 `tween` 外，还需要指定对应的实例。所以经常会出现以下的错误用法: 

```javascript
// 不报错但是不生效
createjs.Tween.get(instance).pause(); 

// 报错
createjs.Tween.pause(tween); 
```
