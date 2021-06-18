## node 环境变量 process.env
- process 对象是一个 global 全局变量，控制当前 node.js 进程
* process.env.NODE_ENV 用来设置当前项目的环境以及在该环境下项目的配置

## node 基础
+ nvm node 版本工具
+ node 中可以控制运行环境，可以编写 node 版本支持的所有 ES6-7-8 JavaScript
+ v8 是 chrome 提供支持的 JavaScript 引擎 ，也为 node 提供支持
+ `process.env.NODE_ENV` ,读取环境变量，默认 development
+ 运行指定 js 文件
  + node app.js
+ console 命令
  + console.trace()  打印函数的堆栈踪迹
  + console.time(), console.timeEnd() 计算函数运行所需要的时间
  + 模块
    + 第一种，只导出一个对象
      + 导出 `module.exports` = obj ,只导出 obj
      + 导入 const obj = reauire('path/obj')
    + 第二种，将导出对象添加为 exports 属性，这样可导出多个对象
      + exports.car = obj
      + const item = require('path/items') ,items.car
+ npm
  + install
    + npm install --save ，安装并添加条目到 package.json 文件中的 dependencies ，与生产环境中的应用程序有关
    + npm install --save--dev ，安装并添加到 devDependencies, 通常是用于开发的库
    + npm install -g 将包安装在全局，默认安装到当前文件树的 node_modules 子文件夹下，并添加到 dependencie
  + npm undate ,检查并更新所有软件包是否有满足版本限制的更新版本

+ `运行任务`
  +  npm run <task-name> 指定命令行任务, 指定要运行的 js 文件命令
    + 在 package.json 文件中 "scripts" 中指定 task-name 要运行的 js 文件
      + "scripts"：{ "dev" : "node lib/src/dev", "prod" : "node lib/scr/build"}
    + 用这个命令来运行 webpack 指定运行环境

## package.json 指南
