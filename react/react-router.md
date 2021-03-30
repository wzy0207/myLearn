- 导入 router 的相关包
  - `import {Router, Route, Link } from 'react-router'`

* 使用 router 配置路由
  - `React.render((<Router><Route path='about' component={About}></Router>))`

- 默认子组件
  - `<IndexRoute conponent={About}>`

* 重定向
  - `<Redirect from="/about" to="/newabout">`

- 进入和离开的 hook
  - onEnter 由外层父路由向内层触发
  - onLeave 由最内层向最外层触发

* React RRouter 建立在 history 上，常见的 history 有三种形式
  - browserHistroy /path,`browserHistory.push('/some/path')`实现组件外部跳转
  - hashHistory /#/path
  * createMemoryHistory

- 路由跳转
  - `<link to="path"></Link>`

### react-router-dom

- 使用 Router 组件包裹根节点实现全局路由访问,必须包裹跟组件
  - BrowserRouter history
  - HashRouter hash
  * `<Router><App /></Router>`
  - 在父组件中确定渲染位置，使用`{props.children}`

* 使用 Route 组件完成路由,类似 vue 的`<router-view>`
  - <Route path="/about" component={About}>

- 精准匹配
  - 在 Route 标签上加上 exact 属性,`<Route path="/" exact component={Home}/>`
  * 使用 Switch,`<Switch><Route 1><Route 2></Switch>`

* 跳转到某个路由--编程式导航
  - `this.props.history.push('/...')`

- query 传参
  - `this.props.history.location.search` 中
  * 动态路由，使用`this.props.match.params`

* 路由书写规则
  - 更具体的路由，带有参数的路由写在普通路由之前
    - `<Route path='/person/:id'>`写在`<Route path='/person'>`之前

- 钩子函数
  - useHistory()
    - 访问历史记录
      - `let history=useHistory()`,`history.push("/home")`
  * useLocation()
    - 保存 URL 信息
      - `let location=useLocation()`,`location.pathname`,`location.search`
  - useParams()
    - 返回 url 的参数键值对
  * useRouteMatch()
    - 尝试以相同方式匹配当前 url
