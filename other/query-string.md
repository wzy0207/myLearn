# `query-string`用来解析url的查询字符产
+ `import qs from 'query-string'`导入库
    + parse()---`qs.parse('?name=jim') `返回`{name:'jim'}`
    + stringify()---`qs.stringify({name:'jim',age:22})`，返回`'age=22&name=jim'`
    + parseUrl()---`qs.parseUrl(''http://www.baidu.com?name=jim'')`,返回`{url:'http://www.baidu.com',query:{name:'jim'}}`