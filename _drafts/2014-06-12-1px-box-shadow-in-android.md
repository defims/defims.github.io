android 2~3支持-webit-box-shadow但需要blur值为>=1px，当然使用特定的其他单位值使得浏览器内部转换为1px也是可以的，但在android4.0+的机子上1px会影响显示，可以利用box-shadow来进行兼容．
代码如下:

    -webkit-box-shadow : 0 0 1px black;
    box-shadow : 0 1px 0 black;
