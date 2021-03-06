## 并发模型与事件循环
+ 事件循环负责执行代码、收集和处理事件以及执行队列中的子任务。
+ 栈，函数调用形成了一个由若干帧组成的栈
    + 函数调用入栈，函数调用其他函数，继续入栈，函数执行完毕，弹出栈，直至栈为空
+ 堆，对象被分配在堆中，堆是一个用来表示一大块内存区域的计算机术语。
+ 一个JavaScript运行时包含了一个待处理的消息队列。每一个消息都关联着一个用以处理这个消息的回调函数
    + 在事件循环的某个时刻，运行时会从最先进入队列的消息开始处理队列中的消息，被处理的消息会被移出队列，并作为参数来调用与之相关联的函数
    + 函数的处理会一直进行到执行栈再次为空为止，然后事件循环会处理队列中的下一个消息

## 事件循环
+ 执行至完成，每一个消息完整地执行后，其他消息才会被执行。一个函数执行时，不会被其他代码终端。
    + 缺点，当一个消息需要太长事件才能处理完毕，web应用程序无法处理与用户的交互
+ 添加消息
    + 当一个事件发生并且有一个事件监听器绑定在该事件上时，一个消息就会被添加进消息队列
    + setTimeout的第二个参数值代表了消息被实际加入到队列的最小延迟时间，队列没有其他消息且栈空，在延迟时间后，消息马上被处理，但是，若有其他消息，setTimeout必须等待其它消息处理完在处理
        + setTimeout 需要等待当前队列中所有消息都处理完毕之后才能执行，即使超出了由第二参数所指定的时间
+ 永不阻塞，JavaScript事件循环模型永不阻塞

## Promise
+ 约定
    + 在本轮事件循环运行完成之前，回调函数是不会被调用的
    + 即使异步操作已经完成，在这之后通过then()添加的回调函数也会被调用
    + 通过多次调用then()可以添加多个回调函数，他们会按照插入顺序进行执行，链式调用
+ 链式调用
    + 在then中抛出的错误会被下一个then的reject捕获或者catch捕获
    + 一旦遇到异常抛出，浏览器就会顺着Promise链寻找下一个 reject 失败回调函数或者由 catch() 指定的回调函数
 + Promise 拒绝事件
    + rejectionhandled 当Promise被拒绝，并且在reject函数处理该rejection之后会派发此事件
    + unhandledrejection 当Promise被拒绝，但没有提供reject函数来处理rejection时，会派发此事件
    + windwo.addEventListener("unhandeldrejection",event => {})
        + 可以监听`unhandeldrejection`事件捕获未处理的错误，阻止未处理信息打印到控制台
+ 在旧式回调API中创建Promise
    + 通过Promise的构造器对就是回调进行封装构建 Promise
+ 组合
    + [func1,func2,func3].reduce((p,f) => p.then(f),` Promise.resolve`)
        + Promise.resolve 是 p 的初始值
        + 相当于 Promise.resolve().then(func1).then(func2).then(func3)
+ 一个已经变成resolve状态的promise传递给then()的函数也总是会被异步的调用
    + 传递到then()中的函数会被放置到微任务队列中，而不是立即执行，只有事件队列运行结束且为空才开始执行
    + setTimeout执行顺序在then()之后
