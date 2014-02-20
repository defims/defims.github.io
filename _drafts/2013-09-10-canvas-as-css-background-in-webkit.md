[canvas as background image](https://www.webkit.org/blog/176/css-canvas-drawing/)在动态控制显示图片时候具有很好的结构化优势，而在android设备上的[兼容性](http://caniuse.com/#feat=css-canvas)在webkit系列表现不俗。
canvas as background image的优势在于

* 可以使用多背景
* 每个背景对应一段javascript代码，方便模块化和换肤
* 背景对应javascript，即可以做到动态更新
* 作为css background-image属性，相应的拉伸、重复等特性也可以沿用
* 使用canvas技术可以有效改善移动设备上图片不清晰的问题
* svg对于设计人员友好，AI可以直接导出svg，svg可以通过[工具]()转换为canvas代码。
* canvas可以做到非常复杂的效果，而且不必担忧icon font只能单色的问题
* canvas的大小由路径决定，在优化上可以有针对性的优化
* canvas可以内嵌入js或html文件，减少请求
* canvas相关的技术可以应用

而其劣势在于

* 路径复杂的小图形的体积会很大
* 位图支持不够
