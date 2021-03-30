- 存在两种 export 方式
  - 命名导出，一个脚本可以暴露多个命名导出
    - `export let name='wangpangpang'`,
      - 使用 `import name from ''` 导入
  * 默认导出,一个脚本只能导出一个
    - `export default let k=12`
    * `import m from ''` , `m=12`,默认导出模块在引用时可以任意取名
