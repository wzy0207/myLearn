+ 配置 prettier，用于格式化代码
    + 在项目的根目录下添加 .prettierrc 或 .prettierrc.js 或 .prettier.config.js 和 .prettierrc.toml 文件
    + package.json 里添加 prettier 字段

+ 配置 eslint -- 用于检查代码，寻找有问题的模式或者代码，不依赖于具体的编码风格，让程序员在编码的过程中发现问题而不是在执行的过程中
    + parserOptions 
        + ecmaVersion ： 支持 ES6 的新语法
        + sourceType : 设置为 "script" (默认) 或 `"module"`（如果你的代码是 ECMAScript `模块`)
        + ecmaFeatures : obj ，表示想使用的额外的语言特性 jsx
    + ignorePatterns 告诉 eslint 忽略特定的目录和文件
    + setting  
        + react 配置 react 相关
    + extends 拓展配置文件
        + eslint-config-alloy 检查工具
        + eslint-config-prettier 不让 prettier 与 eslint 冲突
    + rules 
        + eslint-plugin-react-hooks 阻止 hook 因某些条件`顺序被破坏引起 bug`，参考 react 官网解释，hook 的执行按顺序来
        + require-jsdoc 给函数写说明，像是 lecood 上面的函数解释一样
        + implicit-arrow-linebreak，强制箭头函数体是隐式返回，只有一条返回语句，`隐藏 return 时`的位置
        + function-paren-newline , 强制创建函数时 小括号必须有一致的换行符
        + max-len ,强制一行代码有多少个字符，制表符由几个空格，注释的最大字符数
        + @typescript-eslint/no-require-imports ，进制调用 require
        + react/no-unstable-nested-components   禁止在组件内部创建组件，这个组件是不稳定的
        + eslint-plugin-prettier ，将 prettier 作为 eslint 规则运行，并且作为独立的 eslint 规则进行报告
            + 'prettier/prettier': 'error' 在 rules 中