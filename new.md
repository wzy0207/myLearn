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

* << 左移运算符
  - 9<<2==36
    - 将 9 转为 2 进制后向左移动两位，右边补 0，后转为 10 进制

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

- 小数取整的方法(Math)
  - ceil()--向上取整
  * floor()-- 向下取整
  * round()--四舍五入
  - parseInt()--抛去小数部分，只取整数

* 小数精度(两位)
  - 四舍五入
    - `num.toFixed(2)`,2 为保留的位数
  * 不四舍五入
    - `Math.floor(num*100)/100`,先将小数变为整数后取整然后变为小数
    * 正则匹配
      - `Number(num.tostring().match(/^\d+(?:\.\d{0,2})?/))`，数子转化为字符串进行匹配，匹配到小数点后两位，转为数字类型

- 解构
  - 对象结构使用花括号{}
    - 必须同名变量进行赋值
  - 数组结构使用中括号[]
    - 数组结构可以跳过某些元素或者使用剩余参数捕获
      - [,,third]=arr,此时 third 等于 arr[2]
      * [first,...remains]=arr,remains[]数组包括 arr[1]以后的内容

* 增强版的对象字面量

  - `const new={name,getName(){},['new'+name]:true }` - name,是`name:name`的简写，将作用域中同名变量的值赋给属性 name

  * `getName(){}`是`getName:function(){}`的简写

- getBoundingClientRect()
  - 返回当前元素的大小及其相对于视口的位置

* `0.1+0.2===0.3`--->false
  - 0.1+0.2=0.3000000004,二进制浮点数中的 0.1 和 0.2 不是十分精确
    - 可以使用机器精度判断两个数值是否相等，Number.EPSILON(机器精度)
      - `Math.abs(n1-n2)<Numer.EPSILON`,两个数值的差值绝对值小于机器精度，判断两数字相等

- a=b=c,赋值操作从右到左进行
  - b=c，a=b
  * a.x=b=c,.操作符优先级高，先运算.操作符
    - a.x=[b=c],先运行 a.x 赋值，然后 b=c
  ## 赋值操作会返回等号右边的运算结果
