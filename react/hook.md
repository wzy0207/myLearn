### Hook

- Hook 是一些可以让你在函数组件里钩入 React State 及生命周期等特性的函数

* Hook 使用规则
  - 只能在函数最外层调用 Hook，不能在循环，判断中使用
  - 只能再 react 的函数组件中调用 Hook

- useState()
  - useState() 在组件第一次执行时创建，之后值只能通过 SetCount 函数改变
  * `const [count,setCount]=useState(0)`返回值
    - count 是 useState 声明的 state 变量，
    * setCount 是更新 state 的函数
  - useState 可以在组件中多次调用
  * `setCount(count+1)` +`you click ${count} times` 模板字符串中`${count}`使用 state 变量

* useEffect()副作用函数
  - 告诉 react 组件需要在渲染后执行某些操作
  * 是 componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合
  - 在组件初始化，更新时调用
  * useEffect()可以返回一个清除函数，在组件每次渲染时都会执行

- useCallback（？？？）
- 返回一个 memoized 函数，`const memoizedCallback = useCallback(()=>{dosomeing(a,b);},[a,b],)`
  - 接收两个参数，第一个回调函数，第二个回调函数依赖项数组

* useReducer(???)
  - `const [state,dispatch]=useReducer(reducer,initialArg,init)`
    - 接收形如`function reducer(state,action){new state}`的 reducer 函数喝一个初始的 state 对象
    * 返回 state 值，和 dispatch 方法
    - dispatch 方法调用 reducer，并传参数给 reducer，以 action.引用传入的参数
  * 初始 state 有两种方式
    - 直接指定初始 state,`const [state,dispatch]=useReducer(reducer,{count:0})`
    * 惰性初始化，将 init 函数作为 useReducer 的第三个参数传入，初始 state 被设置为 init(initialprop)
