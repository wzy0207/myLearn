# Rsjs 是一个库，通过使用可观察序列(observable)来编写异步和基于事件的程序。

- fromEvent()
  - ` const observer=fromEvent(window,"scroll")`，将事件转换为可观察对象
  - 将事件转换成 observable 序列
  * `const anotherobser= observer.pipe()`对可观察对象进行操作
- pipe 是对可观察对象的链式处理的另一种语法表现
  - `observer.map().filter().map()===>oberver.pipe(map(),filter(),map())`

* operators
  - `throttleTime(200)`，(节流函数)，200s 接收一次传递过来的值
  * `map()`,同数组中的 map 方法相同
  - `pairwise()`，将前一个值和后一个值作为数组发出，[1,2,3,4,5]===>[1,2],[2,3],[3,4],[4,5]
  * `tap()`,对每个元素执行一个函数，但是不改变源数组，可在 tap()中做记录或者日志，其他不改变元素的操作
