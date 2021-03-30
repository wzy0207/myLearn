- arr.map()生成元素集合，使用 map()方法遍历数组并对数组内容进行渲染
  - `arr=[1,2,3],listItem=arr.map(item=><li>{item}</li>)` 生成 li 标签集合，放入 ul 中传给 ReactDom.render
  * 在生成的元素集合中，需要给没个元素设置 key 属性，帮助 react 识别哪个元素被改变
    - 提取组件时，key 应该保留在数组存在的组件中，key 放在就近的数组上下文中
    * key 值不能被子组件读取
