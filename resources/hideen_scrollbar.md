# 隐藏滚动条

```css
.example::-webkit-scrollbar {
    width: 0;
    height:0;
    display: none;
}
```

但是！

该方法在 iOS 版微信升级到 WKWebView 后有很大的概率无法隐藏滚动条，在旧的 UIWebView 中不存在此问题。

iOS 微信 6.5.3 版本开始支持手动切换 WKWebView 和 UIWebView 的办法：

点击微信会话列表页点击右上角 `+` 号按钮，选择菜单中的 `添加朋友`，在添加朋友界面的搜索框中输入字符串：`:switchweb`，再点击键盘右下角搜索按钮。切换成功后会提示当前使用的内核是 UIWebview 或是 WKWebview。

校验切换方法：

通过命令成功切换到 WKWebview 后，可通过以下方法验证当前网页使用的是否是 WKWebview 内核。
微信内任意入口进入任意网页，在网页加载成功后向下拉动页面（或点击网页右上角菜单按钮），使之显示出地址栏，当地址栏以 `此网页由` 开头即为当前使用WKWebview，若以 `网页由` 则是使用的UIWebview。
