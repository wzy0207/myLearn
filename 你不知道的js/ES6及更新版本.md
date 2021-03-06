##  ES?现在与未来
+ JavaScript 的词法作用域是基于编译器语义的

## 语法
+ 块作用域声明
    + JavaScript 中变量作用域的基本单元一直是 function ，若需要创建一个块作用域，除了函数声明外，最普遍的就是立即调用函数表达式
    + let 声明
        + {} 块作用域，一对 {} 就是一个作用域
        + `let(a = 2, b, c) {}` ,未标准化的形式，显示块作用域，镜像于 var 的那种 let 的声明形式更加隐式
        + var 声明的变量归属于包含它的整个函数作用域，let 声明归属于块作用域，但是直到在块中出现才会被初始化
        + 在 let 声明变量之前访问该变量会出错，暂时性死区 TDZ -- 你在访问一个已经声明但是没有初始化的变量
        + typeOf 在 let 声明的变量之前使用也会报错
        + 将let声明放在作用域最前面，可以避免过早访问的问题
        + for 循环头部的 let 会为循环的每一次迭代都声明一次变量，这个变量会`封闭在迭代产生的块作用域中`,每次迭代都会创建一个块作用域
    + const 声明，声明的值不允许改变
        + const 声明必须要有显示的初始化，因为声明了之后不能改变
        + const 用于创建常量，设定了初始值之后就只读的变量，初始值是引用类型，`引用地址`不能变，值可以变
        + 不可变是对变量的限制，不是对值本身的限制，值是复杂类型，值内容可变
    + 块作用域函数
        + ES6 开始，块内声明的函数，其作用域在这个块内。 {} 内声明的函数，其作用域在这个 {} 内，在 {} 外访问不到，ES6 之前不存在
+ spread/rest
    + `...` 展开运算符
    + ... 用在数组 ( 任何的iterable ) 之前，将数组展开为单独的值
    + 用来扩展数组，相当于 concat(), `var a = [2,3,4], var b = [1, ...a, 5]`, // b -->[1,2,3,4,5]
    + 将函数中多余的参数收集并存放在一个数组中，`function(x,y,...z){}`,从第三个参数开始之后的参数都会被放入 z 数组中
        + 如果没有命名参数， ... 会收集所有参数为数组, function(...arg){} , arg 是一个包含了所有参数的数组，剩余参数
+ 默认参数值
    + function foo(x, y){ x = x || 11 } , 当 x 要传入 0 时会被忽略，因为 0 是假值
    + 第二种， x = (x !== undefined) ? x : 11; 除了 undefined 以外的值都可以直接传入
    + 对于函数参数来说，undefined 意味着缺失
    + ES6 新增为缺失参数赋默认值 `function( x = 11, y = 31){}` 
        + 传入 undefined 会使用默认值，传入 null 不会丢弃，会进行强制类型转换为 0、
    + 默认值表达式
        + 函数默认值可以是任意的合法表达式，甚至是函数调用
        + 函数参数默认值表达式是惰性求值的，它们只在参数的值省略或者为 undefined 的时候调用
        + 函数声明中形式参数是在它们自己的作用域中(包裹函数参数的`小括号`),而不是在函数体作用域中，在默认值表达式中的标识符引用首先匹配到`形式参数作用域`，在搜索外层作用域
            + var w = 1, z=2 function(x=w+1,y=x+1,z=z+1)
                + x=w+1 w 在形式参数作用域中查找 w，未找到，引用外层作用域 w 
                + y=x+1 x找到了形式参数作用域中的 x， x已经初始化
                + z=z+1 , 右边的 z 发现 z 是一个未初始化的参数变量，这里生成 TDZ 报错
+ 解构
    + 数组解构，`对象解构`
    + var [a, b, c] = [1, 2, 3] ,相当于 var a=1,b=2,c=3    
    + var {x:x,y:y} = {x:4,y:6} 相当于 var x= 4,y=6
    + 属性名和要赋值的变量名相同， var { x, y } = {x:4,y:6}
    + 把属性赋值给`非同名属性`时,即给对象属性起`别名`
        + var { x: bar, y : baz } = {x:4;y:6}  ,相当于 bar = obj.x; baz = obj.y
            + x 属性是原值，bar 是目标变量
    + 不只是声明
        + 解构是一个赋值操作，不只是声明，当变量已经声明，解构只用于赋值
        + ({x, y, z} = bar()) ,当只是赋值以声明变量时，必须用 () 把整个赋值表达式抱起来，不这样，左边 {} 会被当作一个块语句而不是对象
        + 赋值表达式左侧并不必须时变量标识符，任何合法的赋值表达式都可以
            + [ o.a, o.b o.c ] = [1,2,3]
            + {x: o.x, y: o.y, z: o.z} = {x: 1, y: 2, z: 3}
            + 在结构中可以使用计算出的属性表达式 
            + 可以不用临时变量解决 交换两个变量 问题
                + [x,y] = [y,x]  ，即可交换两个变量的值
    + 重复赋值
        + 对象解构性值允许多次列出同一个源属性
            + var { a: x, a: y } ={ a: 1 }
                + x  //1, y  //1
        + 解构赋值表达式的`完成值`是所有右侧对象的值
            +  p = { a, b, c } = o 相当于 p = ({ a, b, c } = o)  完成值是`右侧对象` , 即 p = o
            + 解构赋值表达式组成链，每个 = 表达式都是单独的解构赋值，对象是最右侧对象
+ 太多，太少，刚刚好
    + 不需要把存在的所有值都用来赋值，多余的值被丢弃
    + 如果为比解构出的值更多的值赋值，多余的值被赋为undefined
    + 当展开运算符 ... 出现在数组结构的位置，就执行`集合操作`
        + var a = [2,3,4]  ,var [b,...c] = a , b //2, c //[3,4]
    + 默认赋值
        + 数组解构和对象解构都可以提供一个用来赋值的默认值
        + 组合默认赋值和赋值表达式语法
            + var {x, y, z, w：WW =20} = bar() ，bar有w给WW赋值，没有 WW 赋默认值20
        + 嵌套解构
            + 解构的值中有嵌套的对象或者数组，也可以解构这些嵌套的值，解构中套解构
    + 解构参数
        + function foo( [x,y] ) ,foo([1,2]) ,数组解构
        + 参数中展开运算符 ... 用于收集剩余参数
        + 解构默认值 + 参数默认值
            + function foo( {x =10} ={},{y}={y:10})
                + x=10 是解构默认值，只有传入的第一个参数对象中有 x 属性才会赋值，其他情况 x 的值都是10
                + {y} = {y:10} 中 y 默认解构 {y:10} ,  当传入的参数对象没有 y 属性时， y为undefined
        + 嵌套默认： 解构并重组
            + 先把元对象解构并赋值给带有默认值的变量，然后再重组数组，用重新赋值之后的变量重组源数组
+ 对象字面量扩展
    + 简洁属性
        + 定义一个与某个词法标识符同名的属性，可以直接使用变量名，省略属性名
            + var x = 2, y = 3,  o = { x, y }  相当于 o = { x: x, y: y }
        + 简洁方法
            + var o = { x(){}, y(){} }
            + 生成器的简洁方法形式 var o = { *foo() }
            + 简洁方法意味着匿名函数表达式
            + 简洁方法应该只在不需要它们执行递归或者事件绑定/解绑定的时候使用
        + 计算属性名
        + 设定 [[Prototype]]
            + 直接设定对象的 __propo__ 属性
                + var obj = { __proto__ : obj1 }
            + 要为已经存在的对象设定原型，可以使用 Object.setPrototypeOf(0bj2,obj1)
        + super 对象
            + super 只允许在简洁方法中出现，不允许在普通函数表达式属性中出现
            + super 只允许以 super.xxx 出现，不能以 super() 出现
            + super 用于在对象中调用原型链上的属性或方法
+ 模板字面量
    + `hello ${name}` ${name} 会被js引擎自动解析和求值，被 `..` 包围的字符是一个字符串，其中任何 ${..} 形式的表达式都会被立即在线解析求值
    + 插入字符串字面量中的换行会在字符串值中被保留
    + ${..} 内部可以出现任何合法的表达式，包括函数调用，在线函数表达式调用，甚至其他模板字面量
    + 表达式作用域
        +  插入字符串字面量在它出现的词法作用域内，没有任何形式的动态作用域
    + 标签模板字面量 -- function foo(strings,..value)
        + foo`Everytjing is ${desc}！` ,foo 是一个要调用的函数值，可以是任意结果为函数的表达式，或结果为一个函数的`函数调用`
        + string 是一个由所有普通字符串组成的数组，结果为['Everything is', '!']
        + value 是已求值的在字符串字面值中插入表达式的结果 ，即 ${desc} 的结果值
        + 高级用法，同 reduce 方法一起使用
    + 原始字符串
    + 标签函数的第一个参数 strings 保存了所有字符串的原始未处理版本，用 strings.raw 访问
        + String.raw() 传出 strings 的原始版本，不对原始字符串做处理
+ 箭头函数
    + var foo = (x, y) => x + y
        + (x ,y) 是箭头函数的参数列表，当只有一个参数时，包裹的 () 可以省略， `var foo = x => x + 1`
        + x + y  相当于 `return x + y` , 如果函数体只有一个返回表达式，可以`忽略 {} 和 return`
    + 箭头函数的this
        + 在箭头函数内部，this 绑定不是动态的，而是词法的，箭头函数的 this 绑定是包含该函数的`函数作用域或全局作用域`, 箭头函数的 this `只会指向外层函数作用域`，不会指向其他对象
    + 箭头函数除了词法 this ，还有词法 arguments，词法 super，和 new.target
    + 箭头函数适用机制
        + 函数内部没有 this 引用，且没有自身引用(递归/事件绑定/解绑定),且不会要求函数执行这些
        + 内层函数表达式需要调用外层函数，依赖于包含它的函数的 this ，可以使用箭头函数
+ for..of 循环
    + for..of 循环的值必须是一个 iterable (是一个能够产生迭代器供循环使用的对象)
    + 底层实现，for..of 向 iterable请求一个迭代器，然后反复调用这个迭代器把它产生的值赋给循环迭代变量
    + JavaScript 中iterable 的标准内建值包括 Arrays，Strings， Generators， Collections/TypedArrays
    + 普通对象不适用于 for..of 循环，没有默认的迭代器
+ 正则表达式
    + Unicode 标识，ES6+ 正则表达式新的 u 标识，这个标识会为表达式打开 Unicode 匹配
    + 定点标识 -- y
        + ES6 新增标签模式 y 称为定点模式，主要指在正则表达式的起点有一个虚拟的锚点，只从正则表达式的 lastIndex 属性指定的位置开始匹配
        + 定点模式使用 lastIndex 作为待匹配字符串中精确而且唯一的位置寻找匹配，不会向前移动，要么匹配位于 lastIndex 位置上，要么没有匹配
        + 匹配成功，lastIndex 更新为紧跟匹配内容之后的字符的位置，匹配失败，lastIndex 重置为 0
        + 定点定位
            + 使用定点模式重复匹配，需要提前了解输入字符串的结构
        + 定点还是全局
            + 是用 g模式匹配 和 exec() 从 lastIndex 的当前值开始匹配，同时会在每次匹配后更新 lastIndex，这种匹配模式不是从 lastIndex 位置精确匹配，而是会向前移动， y 定点模式不会移动
        + 锚定
            + ^ 表示匹配的起始字符串，而 y 模式只跟 lastIndex 有关，这种情况下，只要 `lastIndex>0`,匹配就会失败
    + 正则表达式 `flags`,source获取要匹配的字符串，flags 获取标识符
        + var re = /foo/ig
            + re.source   //"foo"
            + re.flags    //"ig"
        + var re = new RegExp(re,'ig'),可以指定正则表达式的标识符，并覆盖原有标识符
+ 数字字面量扩展
    + 八进制形式并没有被正式支持， 052是非标准的，var a = 052, a 的值是 42 ，但是 Number("052") 会被强制转换成 52
    + ES6 支持 0o52 形式的八进制， 0`字母o大小写`52 
    + 唯一合法的小数形式是十进制的，八进制，十六进制和二进制都是整数形式
    + `num.toString(n)` 可以将十进制数字转换为对应 n 进制 
+ Unicode
    + Unicode 字符范围包含可能看到和接触到的所有标准打印字符，被称为基本多语言平面(Basic Multilingual Plane, `BMP`)
    + 在 BMP 之外还有符号称为星形符号( astral symbol),ES6 之前 "\uXXXX" 通过 Unicode 转义字符 u 指定JavaScript字符串为 Unicode 字符,在 ES6 之前用 Unicode 转义符标识 astral 需要使用两个特别计算出来的 Unicode 转义字符，JavaScript引擎会把它解释为单个 astral 字符
    + 在ES6中，出现了 unicode 码点转义 var a = "\u{十六进制数}" {} 中可以包含任意多个十六进制字符
    + 支持 Unicode 的字符串运算，字符串长度
        + JavaScript 字符串运算不能感知字符串中的 astral 符号，会单独处理每个 BMP 字符
        + [...astral].length  可得到字符串长度
        + 处理特殊的 Unicode 码点使用 `String#normilize()` 工具,String 原型方法，规范化可以把多个相邻的组合符号合并，规划化并不完美，多个组合符号无法被规范为单个定义好的规范化符号的情况
    + 字符定位 -- 位于位置2出的字符是什么
        + 普通字符串 `chartAt()`
        + ES6 没有支持 Unicode 版本的 charAt()
        + codePointAt() 返回一个 Unicode 编码点值得非负整数， fromCodePoint() 返回使用指定的代码点序列创建的字符串
        + String.fromCodePoint( str.normalize().codePointAt( n )) 返回字符串 str n索引上的字符串值 
    + Unicode 标识符名
        + Unicode 可以用作变量名
+ 符号 -- var sym = Symbol( "this is a symbol" )
    + ES6 引入了新的原生类型 symbol，但是 symbol 没有字面量形式 
        + 不能对 Symbol 使用 new ，他并不是一个构造器，也不是一个对象
    + 传入符号的是对符号的描述
    + 符号的主要意思是创建一个类字符串的不会与其他任何值冲突的值， const EVT_LOGIN = Symbol( "event.login" )
    + 可以在对象中直接使用符号作为属性名/键值，它实际上并不是隐藏的或者无法接触的属性
    + 符号注册
        + var sym = Symbol.fot("instance") ,全局符号注册
            + Symbol.for(..)在全局符号注册表中搜索，查看是否有描述文字相同的符号存在，有就返回，没有创建并返回
        + 避免意外冲突，可以在描述前加上前缀，上下文的信息,var gsym = Symbol.for( "global.instance" )
        + var desc = Symbol.keyFor( gsym )   //"global.instance"  ,Symbol.keyFor 方法用来提取`注册符号`的描述文本
    + 作为对象属性的符号
        + 把符号用作对象的属性，它会以特殊方式存储，不会出现在对象的一般属性枚举中
        + getOWnPropertyNames(obj)  方法获取不到符号属性
        + 要取得对象的符号属性，使用 getOwnPropertySymbols(obj) ，只会返回对象的符号属性
        + 内置符号
            + 内置符号不会注册在全局符号表里
            + 规范使用 @@ 前缀记法指代内置符号 ，@@iterator，@@toStringTag，@@toPrimitive

## 代码组织
+ 迭代器 -- 迭代器是一种有序的、连续的、基于拉取的用于消耗数据的组织方式
    + 迭代器接口包括一个必须的 next() 方法，用于迭代返回数据还有两个可选方法 return(), throw(), return 用于停止迭代器， throw 用于向迭代器报告一个异常，这些方法都返回 IteratorResult
    + IteratorResult 接口是一个对象，包括 value 属性和 done 属性
    + Iterable 接口必须实现 [Symbol.iterator] 属性，用于产生一个 Iterator
    + next() 迭代
        + 调用 iterable 的 arr.[Symbol.iterator]() 方法，会产生一个迭代器
        + 基本值 string 不是 iterable ，但封箱技术自动讲 string 强制转换为 String 对象封装形式
        + 集合提供了 API 方法 entries() 来产生迭代器
    + 可选的 return 和 throw 
        + 在迭代器自动被解释为异常或对迭代器的消耗提前终止，就自动调用 return
        + throw 用于向迭代器报告一个异常，当未用 try..catch 语句捕获抛出的异常时，未捕获的 thorw 异常会异常终止迭代器，捕获的 throw 不会停止迭代器
    + 迭代器循环   
        + for..of 循环用于消耗一个符号规范的 iterable
        + for..of 循环的终止条件是返回结果的 done 为true，即 for..of 循环不会获得迭代器最后的 undefined值
    + 自定义迭代器
        + 实现 next() 方法
        + 构造一个 iterable ，实现 [Symbol.iterator] 方法，返回迭代器本身
    + 迭代器消耗
        + 数组解构可以部分消耗迭代器，展开运算符 ... 也可以消耗迭代器
+ 生成器
    + 生成器不像普通函数那样运行直到完毕，生成器可以在执行当中暂停自身，可以立即恢复执行也可以过一段时间后恢复执行
    + 生成器每次暂停都提供了一个双向信息传递的机会
    + 语法
        + 运行生成器 
            + 像普通函数一样 foo() 还可以传递参数 foo(5,10), 执行生成器并不实际在生成器中运行代码，而会产生一个迭代器控制这个生成器执行代码
            + yield 
                + a = 2 + yield 3 是不合法的   a = 2 + (yield 3) 合法
                + yield 3 和 a = 3 这样的赋值表达式有同样的优先级， b =2 + a = 3 不合法，所以上面的代码不合法，`b = (2 + a) = 3` 相当于这样 = 运算符左边不合法，只能是标识符，不能是表达式
                + yield 的优先级很低，除了展开运算符 ... 和 , 逗号，yield 优先级最低， yield 2 + 3 ，会首先执行 2 + 3 ,再执行 yield ，这种情况，需要使用 () 包裹 (yield 2) + 3
                + yield 和 = 赋值一样，也是`右结合`的,从右到左执行
            + yield * ，yield委托
                + yield * 需要一个 iterable，然后调用这个 iterable 的迭代器，把自己的生成器控制委托给这个迭代器，直到它耗尽
                + yield.. 表达式的完成值来自于 it.next(..)传递的值，yield *.. 表达式的值是被委托的迭代器的返回值
                + yield * 可以委托自身进行递归
        + 迭代器控制
            + `for(var v of foo())` , for..of 需要 iterable，生成器函数自己并不是一个 iterable ，需要调用才能得到一个迭代器
            + next() 比 yield 多一次是用来启动生成器
        + 提前完成
            + return() 强制迭代器完成，并传递给生成器要返回的值，执行 return 会触发 try 的 finally 语句
            + 生成器每次调用都产生一个全新的迭代器，多个迭代器可以附着在同一个生成器上
            + thorw() 向迭代器抛入异常，若迭代器内未捕获会传给调用代码
        + 错误处理
            + 生成器内部有处理错误逻辑，处理抛入的异常后继续执行生成器代码
            + 生成器内部向外抛出异常需要 try..catch 捕获
            + 错误也可以通过 yield * 委托在两个方向上传播
        + Transpile 生成器
            + 手动实现生成器 switch..case..state
        + 生成器使用
            + 产生一系列值
            + 顺序执行的任务队列
+ 模块
    + ES6 模块
        + import export
        + export，模块导出不只是值或者引用的普通赋值，实际上导出的是对这些东西的绑定
            + export function foo(){} ，export var a =2 
                + 命名导出
            + export { foo as bar } ，重命名导出时，只有 bar 可以导入
            + 每个模块定义只有一个 default ，默认导出
                + `默认导出的值不会在之后修改中改变`，导出时是什么就永远是什么，命名导出的导出值随该值变化而变化
        + import 导入
            + 导入命名导出 import { `foo`, `bar` } from 'foo' ,foo 和 bar 必须匹配 foo
            模块中的命名导出，可以对标识符重命名 import { foo as exp } from 'foo'
            + 默认导出不需要使用 {} 包围， import foo from 'foo'
                + 命名导出必须使用 {} 导入
            + import default { foo, bar } from 'foo' ，命名导出和默认导出可以一起导入
            + import * as foo from 'foo' 导入模块中的所有导出，使用 `foo.default` 调用默认导出
            + 所有导入的绑定都是不可变的，不能重新赋值
            + import 导入的声明是提升的，会被提升到当前作用域的顶层
        + 模块依赖环，import 解析 ，A 导入 B ，B 导入 A
            + 加载模块 A ，扫描该模块所有的导出，这样可以注册所有可以导入的绑定，然后处理 import.. from .. 'B'
            + 对 B 的导出绑定进行同样的分析，当分析到 import..from 'A'时，引擎已经了解 A 的 API，验证 A 的 import 是否有效，然后验证等待的 A 模块中导入 B 的有效性
+ 类
    + class
        + class Foo { constructor( a, b ){}}, constructor 指定 Foo 函数的签名以及函数体内容
        + 类方法不可枚举
        + class 定义内部不用逗号分隔成员
        + 用 class 定义的函数必须用 new 调用，也不存在提升，也捕获创建同名全局对象属性
    + extends， super
        + extends 将子构造器的原型委托到父构造器上
        + super 指向父构造器，super() 调用父类构造函数， super.get() 调用父类的原型方法
        + super 是静态绑定的，不会随调用对象改变
    + 子类构造器
        + 构造器并不是类必须的，省略会自动提供默认构造器
        + 子类默认自动调用父类并传递所有参数，子类构造器中`调用 super 之后才能访问 this`
    + 扩展原生类
        + 构造原生类的子类得以实现
    + new.target -- 元属性
        + new.target 在所有函数中都可用，总是`指向 new 实际上直接调用的构造器`,不论定义在哪
        + 普通函数为 undefined
    + static
        + 静态函数只能用类名调用，实例对象不能调用
        + Symbol.species 
            + 父类构造新实例不想使用子类构造器本身，这个功能使得子类可以通知父类应使用哪个构造器
            + 即设定构造出来的实例是谁的实例，不设定是该构造函数，设定了之后为设定值的实例
            + 用 `instanceof` 判断子类构造出来的是父类的实例

## 异步流控制
+ Promise
    + Promise 不是对回调的替代，Promise 在回调代码和将要执行这个任务的异步代码间提供了一种可靠的中间机制来管理回调
    + Promise只能被决议一次，之后不可变
    + fulfilled 内部的异常不会被同层次的 reject 捕获，因为同层的 reject 是捕获上一个 promise 的决议的，此时，promise 已经决议到 fulfilled ，不能被更改
+ Thenable  
        + promise.resolve() 用来解决 thenable 的信任问题
    + 生成器 + promise 是更好的异步流控制表达方式

## 集合
+ TypedArray
    + 大小端，大端高位字节在前，小端高位字节在后
    + 多视图  
    + 带类数组构造器
+ Map -- var m = new Map()
    + 对象不能使用非字符串值作为键
    + m.set(),m.get() 用来设置和获取键值对
        + m.set(key,value),m.get(key)
        + m.delete(key) ，删除
    + 可以在 Map 构造器中指定一个 键值数组的数组 [[key1,value1],[key2,value2]]
    + Map 值
        + m.has(value) 返回 boolean 确认集合中是否有这个值存在
    + WeakMap   
        + WeakMap 只接受对象作为键，这些对象是弱持有的，值不是弱持有的
    + Set -- var s = new Set() ,是一个值的集合
        + s.add(value) 向集合添加一个值
        + s.has(value) 判断集合有无该值
        + s.entries() 迭代器返回一列项目数组，数组的两个项目都是唯一 set 值
            + [ [value1, value1], [value2, value2]]
        + WeakSet 弱持有它的值，当它的值是该引用对象的唯一引用，在引用改变时，引用的对象被回收
            + WeakSet 的值必须是对象

## 新增 API
+ Array
    + Array.of(1,2,3) 创建数组，传入单个值时，创建有一个值的数组, [1, 2, 3]
        + Array.of(3)  [ 3 ]
    + 静态函数 Array.form() 产生新数组
        + 参数为 iterable 时，使用这个迭代器产生值并复制到新产生的数组
        + 永远不会产生空槽位，Array.from({length:4}) 生成长度为4，值都为 undefined 的数组
        + 第二个参数是一个函数，对数组的每个值进行操作，像 map 一样
    + 创建数组和子类型
        + from， of 都使用它们的构造器来构造数组，创造的实例是调用它们时的构造器的实例，`不会更改`
    + 原型方法 copyWith(target, start, end)
        + target 为要复制到的索引，start 时开始索引(包含)，end 不包含
    + fill() 方法 填充数组
    + find() 方法，返回匹配要求的元素，参数是函数，要找的元素符合的要求
    + findIndex(), 需要严格匹配索引值，使用indexOf , 需要自定义匹配的索引值，使用findIndex
    + entries()、value()、keys()、 [1, 2, 3]
        + entries() 返回键值对数组组成的数组 [ [0, 1], [1, 2], [2, 3]]
+ Object
    + Object.is()   
        + Object.is(NaN, NaN) 返回true
        + Object.is(0, -0) 返回false
    + Object.getOWnPropertySybol()
    + Object.setPrototypeOf(obj1, obj2) 将 obj1 的原型委托的 obj2
    + Object.assign(target, obj1, obj2)  obj1, obj2 的自己拥有的可枚举键值通过 = 赋值被复制到target 中
+ Number
    + Number.parseInt(), Number.ParseFloat()
    + Number.isNaN() 判断是否为 NaN
    + Number.isFinite()
    + Number.isInteger() 
+ 字符串
    + String.raw `` 与标签模板字面值一起使用，获得不经任何转义的原始字符串
    + repeat() ,str.repeat() 用来重复字符串

## 元编程 -- 指操作目标是程序本身的行为特性的编程
+ 函数名称
    + 立即执行函数没有 name
    + window.foo = function(){} 没有 name
    + new Function() 创建的函数 name 为 anonymous 
+ 元属性
    + 元属性以属性访问的形式提供特殊的其他方法无法获取的元信息
+ 公开符号 -- 提供专门的元属性
    + Symbol.iterator 迭代逻辑
    + Symbol.toStringTag 与 Symbol.hasInstance
        + Symbol.toStringTag 制定了在 [Object ___ ] 字符串化时的值
        + Symbol.hasInstance 方法接收实例对象，通过返回 true 或 false 来指示这个值是否可以被认为是一个实例，指定实例 instanceof 的判定条件
    + Symbol.species
        + 控制实例是哪个构造器的实例，虽然是这个构造器生成的实例，但是可以指定该实例是其他构造器的实例
    + Symbol.toPrimitive
        + 用在对象为了某操作必须被强制类型转换为一个原生类型值的时候
    + 正则表达式符号
        + @@match
        + @@repalce
        + @@search
        + @@split
    + Symbol.isConcatSpreadable
        + 用来指示如果把他传给一个数组的 concat() 是否应该将其展开
    + Symbol.unscopables
        + 用来指示使用 with 语句时该对象的哪些属性可以暴露为词法变量， true 时，不可用于 with 作用域
+ 代理
    + 代理是一个特殊对象，它封装另一个普通对象，或者说挡在这个普通对象的前面
    + 可以在代理中注册特殊的处理函数，将操作转发给原始目标还可以执行额外的逻辑
    + 代理局限性 -- 下列操作不会由代理拦截
        + typeof
        + string
        + obj + ""
        + obj == pobj
        + obj === pobj
    + 可取消代理
        + Proxy.revocable( obj, handlers) 返回一个对象，一个代理对象，一个取消方法
    + 使用代理  
        + 代理在先，首先与代理交互
        + 代理在后，将代理对象放到主对象的原型链上，主对象上无法实现的逻辑放到代理对象逻辑中
        + 没有这个属性和方法，代理中处理属性访问逻辑，主对象没有该属性，抛出自定义异常
            + 此时使用代理在后逻辑较简单，只要访问到代理，就抛出异常，不用特殊逻辑
        + 代理 hack 原型链
            + 给对象设定一个符号指向要设定的原型链对象，在代理中设置逻辑，对象本身无属性寻找自定义的原型链对象
            + 伪装多个原型链对象，设定符号为要设定的原型对象数组，在代理中遍历数组进行查找
+ Reflect API
    + Reflect 对象是一个普通的对象，持有各种可控的元编程任务的静态函数
    + 属性排序
        + Reflect.ownKeys()，以及扩展的 Object.getOwnPropertyNames() 和Object.getOwnPropertySymbols() 的顺序是可预测且可靠的
            + 首先，按照数字升序排序
            + 其次，按照创建顺序枚举其余字符串属性
            + 最后，按创建顺序枚举符号属性
    + Reflect.enumerate(), for..in ，遍历自身和原型链， Object.keys() 和 JSON.stringify()，只遍历自身
+ 特性测试
    + 分批发布技术，测试环境能否支持 ES6 ，不支持加载使用 transpile 代码，支持使用 ES6 代码
    + FeatureTests.io
+ 尾递归调用
    + 非尾递归调用导致递归栈溢出
    + 尾调用是一个用 return 函数() 结束的函数，除了调用后返回其返回值之外没有任何其他的动作
    + `非尾调用优化` -- trampolining
        + 相当于把每个部分用一个函数标识，这些函数返回另外一个部分结果函数，或者返回最终结果，然后循环直到得到的结果不是函数
    + 自适应代码
        + 使用 while 循环包裹 try..catch 语句，捕获异常后不处理异常，继续循环直到条件结束

## ES6 之后
+ 异步函数
    + async await
    + 没有办法从外部取消一个正在运行的 async function 实例
+ Object.observe()
+ 幂运算
+ 对象属性与...
    