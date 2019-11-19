## vue 高级技巧

###  **1.css 局部样式** 

- vue 中style样式添加scoped属性表示它的样式只作用于当前组件，样式私有化。
- 但是当前组件的根组件会受到父组件样式的影响。（比如父组件有个.box{background:red}, 如果子组件即当前组件的根组件类名也为box，那背景也会为red）
- 其中渲染的原理： 
  - 给HTML的DOM节点添加一个不重复的data属性 来表示唯一性 
  -  在对应的css选择器末尾添加一个当前组件的data属性选择器来私有化 

- 如果使用了scoped属性后，想让组件内部样式被外部控制，只需要css添加 `deep`属性即可 

```less
<style lang="less" scoped>
.text-box {
  /deep/ input {
    width: 166px;
    text-align: center;
  }
}
</style>
```

###  **2. Vue.filter 全局过滤器** 

- 过滤器的目的主要是为了对数据进行转换
- computed计算属性也可以转换，**但是computed不能接受参数，**只能针对某一个Vue内部属性进行转换，但是filter可以接受参数。

*注册*

 **全局过滤器**

```javascript
Vue.filter('globalFilter', value=>{....})
```

**组件过滤器**

```javascript
filters:{
  testFilters: value=>{...}
}
```

> 注：一直不怎么会用全局过滤器，有关全局的都不清楚，这要重点研究一下这里的知识



使用

 **在双花括号插值** 