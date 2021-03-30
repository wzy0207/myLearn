### react 基本使用方法

- 在 react 中，父组件向子组件传参数在父组件的子组件标签中，使用属性向子组件传值，<child value={i} />,在子组件中，使用 props 接收这个值，以{this.props.value}使用传入的参数

* 在组件中的构造函数中，必须以 super(props)开头，"this.state={value:null}"存放数据，在方法中以 this.setState({value:"X"})来修改 state 中的值

## react 知识点

- npx create-react-app my-app 创建一个新的 react 项目

* 一个 jsx 标签内没有内容，可以使用/来闭合<squares />

- react 函数组件定义的两种方式
  - `const Example=(props)=>{ retrun <div/>}`
  * `function Example(props){return <div/>}`

* react 组件

  - 组件名称首字母大写，小写会被识别为 html 标签

  * `const element = <Welecome name="nanhanhan" />;` 传参数用属性
  * `function Welecome(props) { return <h1>hello,{props.name}</h1>; }` 调参数用 props.属性名

  - 函数组件传参，使用 props 接收参数，在调用组件时，以属性形式传递参数，在函数组件中，用 props.属性名 调用参数

* react 组件生命周期分为三个状态
  - Mounting 已插入真实 DOM
  - Updating：正在被重新渲染
  - Unmounting：已移除真实 DOM

- 组件生命周期的方法
  - componentWillMoint() 在组件渲染前调用
  * componentDidMount() 在组件第一次渲染后调用
  - componentWillReceivePorps() 在组件接收到一个新的 prop 调用，初始化 render 时不会调用
  * componentWillUpdate() 在组件接收到新的 props 或者 state 但还没有 render 时被调用，初始化被会调用
  - componentWillUpdate() 在组件更新完成后立即调用，初始化不会调用
  * componentWillUnmount() 在组件从 DOM 中移除前调用

* react 执行顺序
  - 调用 ReactDOM.render()中传入的组件
  - 调用组件构造函数，初始化 state
  - 执行组件 render() 方法
  - 更新 DOM 匹配组件
  - 调用 componentDidMount() 方法
  * 调用 setState()方法后，react 重新调用 render(),渲染 DOM

- state
  - this.state 只能在构造函数中使用，修改 state 的值使用 this.setState()
  * state 的更新可能是异步的，在同时用到 state 和 props 时要将 state，props 作为 this.setState((state,props)=>({}))的参数
  - state 中的数据可以传递给子组件

* react 事件
  - react 事件调用`<button onClick={handleClick}>` 事件命名小驼峰 onClick，{函数名}添加事件
  * react 事件不能通过`return false`阻止默认行为，只能通过`e.preventDefault()`阻止
  - react 事件函数必须在构造函数中绑定 this,`this.handleClick = this.handleClick.bind(this);`
    - class 中的方法不会绑定 this，未绑定时，this 的值未 undefined
    * 两种不用 bind 的方法
      - 使用箭头函数调用方法`onClick={()=>this.handleClick()}`
      * 使用实验性语法， 定义函数时 `handleClick=()=>{}`,`onClick={this.handleClick}`
  * 像事件函数传递参数,删除某行
    - `onClick={(e)=>this.handleClick(id,e)}`
    - `onClick={this.handleClick.bind(this,id)}`,事件对象隐式传递
