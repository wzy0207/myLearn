### TS

- 给函数的参数添加类型注解
  - `function hello(name: string){}`，约束传入的参数是字符串类型
- `interface Person{}`定义一个接口，约束参数数组的属性

### TS 数据类型除了 JS 常有的之外，还有一些扩展

- any,当不清楚变量类型的时候，使用 any 通过编译阶段的检查
  - any 类型的变量在定义时可以使用方法，表示可能会发生
  - void 表示没有任何类型，当一个函数没有返回值时
  - object 表示非原始类型，如对象，但不访问任何键
  - `Record<any, any>`，允许访问任何键

### 接口就是定义一个类型，对参数变量进行约束

- `interface Person{name :string }`要求传入的参数的 name 属性必须是字符串类型
- 可选属性`interface Person{name?:string}`name 属性存在与否是可选择的，可传可不传
- 只读属性 `interface Point{readonly x:number}`定义了只读属性的变量值不能被修改
- 函数接口，
  - `interface FunctionName{(name:string,age:number):boolean}`，第一个参数定义函数接受参数的属性类型，第二个参数指定函数返回值的类型
  - `interface Myfunc{ function1:(name:string)=>void}`
    - ()指定参数是函数，箭头指定参数返回值时 void

### 类型断言，清楚的知道一个实体更确切的类型，两种方式

- 尖括号，<any>该变量的值类型是 any
- as ,`somevalue as string `某个值类型是 string

### 模块

- 模块导出
  - 任何声明都能通过 export 关键字导出，`export interface MyInterface`
- 模块导入
  - `import {MyInterface} from 'interface'`

### 注释

- `// @ts-ignore`忽略.ts 文件中报错行的错误，在报错行上方使用
