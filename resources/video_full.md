# 微信下 iOS Android 全屏视频

在制作全屏视频需求时，你会发现 iOS、Android 在这一块多多少少都存在一些问题。

**iOS**

iOS 通过 `webkit-playsinline`、`playsinline` 实现内嵌全屏，不调用系统全屏，有必要的情况下配合 [iphone-inline-video](https://github.com/bfred-it/iphone-inline-video) 库使用。

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

**Android**

Android 除非你的域名下微信的白名单，否则你会面临两种选择：

1、使用 X5 自建播放器
2、隐藏导航条的全屏视频

自建播放器就跟废的没两样，全屏偶尔还能用上，通过 `x5-video-player-type="h5"`、`x5-video-player-fullscreen="true"`可视频全屏，同时顶部的微信标题条也会消失，视频内会有一个全屏的 icon 按钮，退出全屏会使视频暂停

```html
<video src="video.mp4" x5-video-player-type="h5" x5-video-player-fullscreen="true"></video>
```
