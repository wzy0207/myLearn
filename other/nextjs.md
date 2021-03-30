### 全局样式表的导入

    + 在`pages/_app.js`文件中导入`global.css`,`import global.css`
    + `pages/_app.js`必须有默认导出函数`export default function MyApp({ Component, pageProps }) {

return <Component {...pageProps} />
}`

### page 根据文件名与路由关联，`pages/about.js`被映射到`/about`

    + 默认打开url地址返回的是index页面
    + 在pages文件夹下的路由文件，要使用`export default 文件名`将文件暴露出去
    + 动态路由：`pages/posts/name.js`,通过`posts/name`来访问

- 两种形式预渲染
  - 静态生成：HTML 在构建时生成，每次页面请求时重用，两种请求方式在页面构建时调用
    - 页面内容取决于外部数据，使用`getStaticProps`
    - 在组件导出一个异步函数，用于获取数据`export async function getStaticProps() {}`，并通过`return {props:{posts,}}`对象将数据作为`props`的参数传递给页面
    - 页面路径取决于外部书数据，使用`getStaticPaths`
    - 在组建内导出一个异步函数，创建路径列表，再根据路径列表获取相应的数据
  - 服务器端渲染：每次页面请求时重新生成 HTML，动态渲染
    - 在 page 使用服务器端渲染，需要导出一个异步函数`export async function getServerSideProps() {}`,服务器每次页面请求时调用此函数，构建时不运行

### 路由

- pages 文件夹下的每一个 js 页面都是一个路由
- `<Link href="/about"></Link>`用于页面之间的跳转
- 页面导航`useRouter()`
  - `const router=useRouter(),router.push('about')`
- useRouter 的返回值
  - pathname，当前的路由地址
  - query，当前 url 的查询参数
