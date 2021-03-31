- `const uuidv4 = require('uuid/v4')`,`uuidv4()` 方法用于生成一个理论上不重复的 128 位 16 进制的代码
- `navigator.userAgent` 声明了浏览器用于 HTTP 请求的用户代理头的值
- `numberObject.toFixed(num)`,`num.toFixed(1)` 把 num 四舍五入为 1 位小数的数字
- process 是 node 中表示当前 node 进程的全局变量
  - `if(process.browser)`判断是否为客户端
  - `process.env`包含着关于系统环境的信息
    - `process.env.NODE_ENV`用户自定义变量，用来判断当前是开发环境还是生产环境

### 运算符

- ??判断符号
  - `process.env.NODE_ENV ?? 'development'`,判断问号前值是否为空，为空赋予后面的值，不为空跳过，取原来变量值
- ?. 可选链操作符
  - 允许读取位于连接对象链深处的属性的值，当引用为空不报错，返回 undefined

* &&
  - 表达式 1 && 表达式 2
  - 表达式 1 结果为 true，返回表达式 2
  * 表达式 1 结果为 false，返回表达式 1
* ||

- 表达式 1 || 表达式 2
  - 表达式 1 结果为 true，返回表达式 1
  * 表达式 1 结果为 false，返回表达式 2

### Date()对象的方法

- new Date().toLocaleDateString() 将 date 对象的日期部分转换为字符串--->2021/3/25

* new Date().toLocaleTimeString() 将 date 对象的时间部分转换为字符串--->下午 2：31：20

### ++i，i++

- i++
  - `for (var i=1,i<3,i++)`,第一次循环，i++=1,i=2
  * i++ 返回的是没有执行运算前的值

* ++i
  - ++i=2;i=2
  * ++i 返回执行后的值
