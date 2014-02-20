在桌面浏览器上对:hover和:active伪类支持是比较完善的，但移动设备交互形式不同，对:hover和:active的支持也就不尽人意了。
###:hover在webview上的效果是点击后不恢复，直到点击下一个，相当于:focus
###:active需要在对应元素或父元素上增加`ontouchstart=''`来唤醒

而使用javascript的polyfill方案是利用touch事件动态添加hover或者active类

<script src="https://gist.github.com/defims/9116800.js">
</script>
