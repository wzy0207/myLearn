## 关于this
+ 为什么用this
    + this提供了一种更优雅的方式来隐式传递一个对象的引用
+ 关于this的两个误解
    + this指向自身
    + this指向函数作用域
+ this是什么
    + this的绑定和函数声明的位置没有关系，只取决于函数的调用方式（调用位置，调用方法）


## this全面解析
+ 调用位置
    + 函数在代码中被调用的位置
+ 绑定规则--4种
    + 默认绑定
        + 函数是直接使用`不带任何修饰的函数引用`(解释在其他函数体内调用函数时的this也是默认绑定)进行调用的，执行默认绑定
        + 非严格模式下绑定到window，严格模式下绑定到undefined，`---函数体内运行规则，不是函数所在作用域运行规则`
    + 隐式绑定
        + 隐式绑定最重要的规则是函数是否是对象属性调用，只要不是以obj.foo()调用，都是默认绑定
        + 调用位置是否有上下文对象，是否被对象调用，对象包含指向函数引用的属性
            + obj.foo()，此时foo函数的this指向obj
        + 对象属性引用链只有最后一层会影响调用位置
            + obj1.obj2.foo(),此时foo函数中的this绑定在obj2上
        + 隐式丢失
            + `obj = {foo:foo},function foo(){},var bar=obj.foo,bar()`,
            + 变量赋值为对象属性对函数的引用，变量是函数的引用，不带修饰符调用时函数this绑定在全局对象
            + 回调函数中的函数调用也会发生隐式丢失，执行默认绑定
    + 显示绑定
        + call(),apply()---`foo.call(obj)`
            + 显示绑定无法解决丢失绑定问题
        + 硬绑定，bind(),使用bind()绑定会返回一个新函数，除了new操作符，返回的函数的this绑定不会被其他操作改变。
            + bind()实现，bind返回一个新函数，函数中对传入的函数参数进行显示绑定
            + bind()实现柯里化
        + forEach(fn,obj)，forEach的第二个参数指定fn的this绑定对象
    + new 绑定,`var obj = new Base()`
        + 使用new调用的函数会返回一个新对象
        + new调用函数，执行如下操作
            + 创建一个全新的对象 `var obj = {}`
            + 对这个对象执行原型链链接 `obj.__proto__ = Base.prototyoe`
            + 将新对象绑定到函数调用的this `Base.call(obj)`，即对新对象进行初始化
            + 如果函数没有返回一个`对象`(返回基本数据类型会被忽略),返回新创建的对象 `return obj`
+ 优先级
    + 默认绑定优先级最低
    + 显示绑定高于隐式绑定
    + new 不应该比较优先级，因为new返回的是一个新对象，不会对函数之前绑定的对象进行修改
    + new可以改变硬绑定的this绑定
+ 判断this
    + new绑定
    + 显示绑定
    + 是否是上下文对象调用，obj.foo()
    + 默认绑定
+ 被忽略的this
    + 显示绑定传入的绑定对象为null或者是空对象时，this被忽略
    + object.create(null)创建一个比{}还空的对象，不会执行原型操作，用于忽略函数this绑定
+ 间接引用
    + 赋值表达式返回的时目标函数的引用
+ 软绑定
    + 当函数的this为默认绑定时，指定函数this为一个特定的对象
    + 不是默认绑定，则应用当前绑定规则
+ this词法
    + 箭头函数中的this不会被改变，new操作符也不会改变箭头函数的this指向
    + 箭头函数的this根据外层(`函数或者全局`)作用域来决定
    + 箭头函数根据当前的词法作用域来决定this的指向


## 对象
+ 语法
    + 定义对象的两种方式
        + 声明形式，文字形式`var myobject = {}`
        + 构造形式， `var myobject = new Object()`
        + 区别，文字形式定义的对象可以添加多个键值对，构造形式只能逐个添加
+ 类型
    + 7种类型
        + 基本类型，`string,number,null,boolean,undefined`
        + 引用类型，object
        + es6,symbol
    + `typeof null`返回object是语言的bug，null是基本类型,原理不同对象底层都表示为二进制，js中前三位都为0判断为object类型，null的二进制表述是全0，null被判定为object
    + 内置对象
        + String,Number,Boolean,Object,Function,Array,Date,RegExp,Error
        + 用字面量形式创建的基本类型变量，类型为基本类型之一,`没有原型,不存在原型链`
        + 用new创建的内置函数，类型是对象，`new 返回的是对象`
        + 字面量在执行对象方法时，js自动将其转化成该类型的对象
        + null和undefined没有对应构造形式，Date只有构造形式
+ 内容
    + 对象的内容是由一些存储在特定命名位置的值组成的
    + 对象容器内存储属性名，属性名指向值存储的地址
    + 属性访问
        + myobject.a  -- 属性访问
        + myobject["a"]   -- 键访问
        + 区别：属性名称不满足标识符的命名规范时，必须使用键访问，--myobject['super-fun']
        + 在对象中，属性名永远都是字符串，使用字符串以外的值类型作为属性名是会先被转换为string类型，数字也不例外
    + 可计算属性名--`-var myobject = { [a+b] : "hello"}`,[变量表达式]可以作为属性名，表达式执行完后转为string
    + 属性与方法
        + 对象中不存在方法，因为函数的特性，函数不会属于任何一个对象，只能作为属性引用，不能叫做方法
    + 数组
        + 数组也是对象，数组中的元素和数组的属性不是一个东西
        + `arr.a="123"`,不会改变数组内部元素，只会给数组添加一个属性
        + 数组通过数字下表获取内部元素，`arr["4"]='123'`,会将数组下标4改为"123",`arr["a"]`会给数组添加一个名称为a的属性 
    + 复制对象，=赋值时
        + 对基本数据的复制都是深拷贝
        + 引用数据类型的复制是浅拷贝
            + 引用数据类型可以使用遍历将引用数据类型的所有基本类型进行深拷贝
        + `Object.assign({},myobject)`,方法实现浅拷贝，=赋值的形式实现
    + 属性描述符,对象中的所用属性都具有属性描述符
        + value ，属性的值
        + writabe 描述值是否可更改
        + enumerable 属性是否可枚举
        + configurable 属性是否可配置，修改属性描述符，false时可修改writable由true为false，不能将writable由false变为true，不能删除对象
        + `Object.defineProperty({},"a",{value:2,writable：true})`方法添加一个新属性并对属性配置进行设置
    + 不变性
        + 所有的方法创建的都是浅不变性，只=只会影响目标对象和直接属性，如果目标对象引用了其他对象，引用的对象不受影响，和`const`一样
        + 对象常量，属性描述符，`writable,configurable`
        + 禁止扩展，`Object.preventExtensions(object);`,禁止添加属性
        + 密封，`Object.seal()`,不能添加新属性，也不能重新配置或者删除任何现有属性
        + 冻结，`Object.freeze()`,在seal基础上writable：false，最高级别不可变性
            + 深度冻结，目标对象冻结，遍历目标对象，将所有引用对象冻结
    + [[Get]]
        + `myobject.a`在myobject上实现了[[Get]]操作，查找该都对象上是否有同名属性，有返回，没有查找原型链
        + 没有找到返回undefined
    + [[Put]]
        + [[Put]]被触发时，首先检查对象是否存在这个属性
            + 属性是否是访问描述符`（当一个属性定义了getter和setter中的任意一个）`,是且存在setter，调用setter
            + 属性的数据描述符writable是否是false
            + 都不是后设置属性的值
    + Getter和Setter
        + 部分改写默认操作，只能应用在单个属性，不能应用在整个对象
        + 当一个属性定义了getter或setter之后，js会忽略它的value和writable特性，转而关注set和get
        + 给属性前加上get和set标识符设置getter和setter
        + 当某属性只定义了getter，对属性赋值无效
        + setter会覆盖单个属性默认的[[Put]]操作
    + 存在性
        + `a in myobject`,in检查属性是否对象或者对象的原型链上
        + `myobject.hasOwnProperty("a")`，hasOwnProperty只会检查属性是否在myobject中，不会查找原型链
        + 通过`Object.cteate(null)`创建的对象没有原型，无法通过Object.prototype访问hasOwnProperty()方法
            + 通过`Object.prototype.hasOwnProperty.call(myObject,'a')`使用
        + 枚举属性--属性是出现在对象的遍历中
            + `Object.key()`返回对象中所有可枚举属性
            + `myobject.propertyIsEnumerable("a")`,检查给定属性名是否在对象上且可枚举
            + `Object.getOwnPropertyNames(object)`,返回对象中所有属性名，无论是否可枚举
+ 遍历
    + 迭代，for in
    + `for( var i of arr ){}`,for of 遍历数组所有元素
        + 迭代器实现，向对象请求一个迭代器，通过调用迭代器对象的next方法来遍历
    
## 混合对象类
+ 类理论
    + 面向对象编程强调的是数据和操作数据的行为本质上是互相关联的
    + 将数据和相关行为封装起来---类实例
    + 类，继承和实例化，交通工具类，继承类汽车，实例：有车牌的汽车
    + 多态，父类的通用行为可以被子类用更特殊的行为重写
        + 相对多态，从重写行为中引用基础行为
    + 类是一种设计模式
        + 类通过复制操作被实例化为对象形式
        + 类实例是一个特殊的类方法构造的，这个方法名通常和类名相同，构造函数，任务是初始化实例需要的所有信息
        + 类构造函数属于类，通常和类同名，构造函数会返回一个对象
+ 类的继承
    + 多态，子类重写父类的同名方法
        + 相对多态，子类重写父类同名方法，并在子类中调用父类的的该方法
            + 相对--相对引用，查找上一层，任何方法都可以引用继承层次中高层的方法
        + 在继承链的不同层次中一个方法名可以被多次定义，当调用方法时会自动选择合适的定义
    + super
        + 在子类构造函数中调用父类构造函数，或者是引用父类的某个方法
        + `js中类是属于构造函数的`
    + 方法定义的多态性取决于你是在哪个类的实例中引用它
+ 混入
    + js中类的实现是基于圆形的
    + js中不存在可以被实例化的类，一个对象不会被复制到其他对象，他们会被关联起来（`ptorptype`）
    + js的特性，赋值时复制的时函数的引用
    + 显示混入
        + 目标对象不存在源对象某个属性是，复制该属性到目标对象
        + 绝对引用，`vehicle.drive.call(this)`,相对对台，查找上一层，绝对引用，使用名称显示指定
        + 混合复制
            + 将父类属性全部复制到新对象中
            + 将新属性通过minin函数赋值到新对象上
            + `第一种是当目标对象没有源对象上的属性时复制，第二种是全部复制`,第二种会重写父类方法
        + 寄生继承
            + 将父类的通用方法写在原型链上
            + 子类中构造父类的实例对象，获得父类的所有方法和属性
            + 定义父类实例对象的特殊方法，可以引用父类原型的方法并重写该方法
            + 返回这个实例对象
    + 隐式混入
        + `something.coll.call(this)`,在对象中引用其他对象的方法，不能变为相对引用
+ 小结
    + 类是复制，js中不存在类
    + 多态引用也是复制
    + 对象只能复制引用，无法复制被引用的对象或者函数本身

## 原型--js中只有对象
+ [[Prototype]]机制就是存在于对象中的一个内部链接，它会引用其他对象
    + 作用，如果在该对象没有找到需要的属性和方法，引擎就在关联的[[Prototype]]对象上继续查找，直到查到或者是查完整条原型链
+ [[Prototype]]
    + js中的对象有一个特殊的[[Prototype]]内置属性，对于其他对象的引用
        + 引用对象属性时，[[Get]]首先检查该对象本身有无此属性，没有则查询原型链
        + 查完整条原型链都没有找到，则返回undefined
        + for..in 遍历也查找原型链上所有可枚举属性，in查找属性在对象中是否存在同样查找原型链，无论属性是否可枚举
    + object.prototype是所有普通原型链的顶端，它的原型指向null
+ 属性设置和屏蔽
    + `myobject.foo='bar'`
        + myobject本身具有属性，直接修改
        + myobject本身不具有该属性，遍历原型链
            + 原型链上没有，foo直接添加到myobject上并赋值
            + 原型链上存在foo
                + foo没有被标记为只读，直接在myobject上添加foo
                + foo标记为只读，赋值操作被忽略，模拟类属性继承
                + foo是一个setter，调用setter，myobject上不添加foo
                + 第二种和第三种情况可以使用，`object.defineProperty()`屏蔽原型上的foo
        + myobject本身有，原型链也有，发生屏蔽，myobject本身的foo屏蔽原型上的所有foo
    + 隐式屏蔽
        + myobject.a++
            + myobject.a = myobject.a + 1,先查找原型获取值，在进行赋值，发生屏蔽
+ 原型对象是函数才具有的，原型是所有对象都有的
    + 原型对象--fun.prototype，引用原型对象
        + 函数的原型存储在原型对象上，通过`func.prototype.__proto__`来访问
        + 对象的原型存储在自身,`obj.__proto__`来访问
        + `Foo.prototype`默认有一个`.constructor`属性，指向函数本身
            + constructor并不表示被构造，它是函数声明时的默认属性，替换函数的prototype对象，新对象不具有constructor属性，需要使用Object.definePrototype手动添加
            + 实例对象没有原型对象也就没有constructor属性，它引用的是原型链上的该属性
            + .constructor是不可枚举的，但是它的值是可以改变的
    + 原型---new foo().__proto__,指向原型链
    + new创建的实例对象通过`__proto__`指向构造函数的原型对象    
+ js中没有复制机制，不能创建一个类的多个实例，只能创建多个互相关联的对象（prototype关联的是同一个对象）
    + new foo()没有直接创建关联，只是一个副作用
    + 直接创建关联，`Object.create()`
    + 原型继承，继承是复制，js中没有复制，只有关联
    + 差异继承，描述对象行为时，使用其不同于普遍描述的特质
+ 构造函数
    + js中没有构造函数，只有构造函数调用,new会劫持所有普通函数并用构造对象的形式调用它
    + 当函数使用`new`时，函数调用会变成构造函数调用，并构造一个对象
+ （原型继承）
    + Bar.prototype = Object.create( Foo.prototype )
        + Object.create创建一个新对象，并将新对象的原型（__prpto__）关联到指定对象
    + Bar.prototype = Foo.prototype
        + 将Foo原型对象的引用赋值给Bar原型对象，修改Bar原型对象会影响Foo.prototype
    + Bar.prototype = new Foo()
        + 使用了Foo的构造函数调用，当Foo有一些副作用，会影响Bar的后代（稍作理解）
        + function Foo(){this.name=name,this.age=age},都会传递给Bar的原型对象
    + 关联原型对象的方法
        + es6之前,`Bar.prototype = Object.create( Foo.prototype )`
        + es6之后，`Object.setPrototypeOf(Bar.prototype,Foo.prototype)`
    + 检查类关系，内省，检查对象的祖先
        + `a instanceof Foo`,instanceof，检查在a的整条原型链上是否有指向Foo.prototype的对象
            + 只能处理对象和函数之间的关系，a对象，Foo函数
            + bind生成的硬绑定函数没有.prototype属性
        + `Foo.prototype.isPrototypeOf(a)`,在a的整条原型链上是否出现过Foo.prototype
            + `b.isPrototypeOf(a)`,判断两个对象的关系
        + `Object.getPrototypeOf(a)`，获取a的原型链
            + a.__proto__,获取原型链，__proto__ 不存在于对象本身，存在于prototype对象上
                + __proto__ 实现：get和set方法，其中分别使用了`getPortotypeOf`和`setPrototypeOf`
+ 对象关联
    + `var b = Object.create(a)`,创建一个新对象，并将新对象的原型设置为指定对象(将新对象关联到指定对象)，即`b.__proto__=a`
        + 避免一些麻烦，用new调用函数会生成不必要的`.prototype`对象
        + 垫片，创建一个空函数，将该函数的prototype属性设置成指定对象，返回这个函数的一个new对象
    + 内部委托让代码设计更加清晰--`委托设计模式`
        + `myobject.doColl = function(){this.coll()}`,myobject没有coll函数，使用的是委托
        + 内部委托比直接委托更加清晰

## 行为委托
+ js中原型链的本质就是对象之间的关联关系
+ 面向委托的设计
    + 类理论，在父类中定义所有任务都有的行为，子类继承父类，并会添加一些特殊行为
        + 类设计模式在继承时重写父类同名方法，实现多态
        + 类是复制，复制父类的行为到子类
    + 委托理论，子对象通过关联委托给父对象，在需要时进行委托，父对象包含所有任务可以使用的具体行为，对每个任务都定义一个对象来存储对应数据和行为
        + `object.create()`创建关联
        + js中没有类似类的抽象机制
        + 对象关联风格的不同之处
            + 在委托设计中最好把状态保存在委托者而不是委托目标上
            + 类模式中故意使用同名方法来利用重写优势，委托行为中避免在不同级别原型链上使用相同的命名，尽量少使用容易被重写的方法名
            + 委托行为意味着对象在找不到属性和方法时会把这个请求委托给另一个对象
            + 委托行为最好在内部实现，不要直接暴露出去
    + 互相委托时禁止的，当引用一个两边都不存在的属性，会在原型链上无限递归
    + 调式--无意义，chrome跟踪对象内部的构造函数名称的方法是一个bug
        + `var a = new Foo()`，a打印Foo{}
        + `var a = Object.create( Foo )` , a打印Object{},或是打印修改后的Foo的constructor值