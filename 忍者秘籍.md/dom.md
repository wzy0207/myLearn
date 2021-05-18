### style

- style 属性中的任何值，都优先于样式表继承的值-即使样式表规则使用!important 注释
- style 属性中不反映从 css 样式表中继承的任何样式信息

* 脚本中，js 代码中，css 属性命名为驼峰式命名，原因是连字符-会被 js 引擎识别为减法运算符

- 脚本中获得 css 所有样式的方法
  - getComputedStyle(element)
  * getPropertyValue(property)--->property 为传入的属性名
