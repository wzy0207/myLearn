+ `encodeURIComponent()` 
    + 该方法用 UTF-8 序列替换某些字符的每个实例来编 URI
    + A-Z a-z 0-9 - _ . ! ~ * ' () , 除了这些字符外，其他的都转义
    + 当 URL 地址中有特殊字符时不会被浏览器解析，需要编码后请求