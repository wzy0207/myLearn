+ `window.open(url)`打开url地址
+ `event.srcElement`是标准的`event.target`属性的别名

+ `window.screen.orientation`,监听移动设备旋转事件
    + `window.orientation` , 也可以，但是不兼容
    + ad_template 项目使用的工具不支持 `window.screen.orientation` 事件, 监听 resize 事件,判断文档高度和宽度的关系，宽大于高 是横屏