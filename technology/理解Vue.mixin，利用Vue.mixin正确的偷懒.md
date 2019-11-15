## 理解Vue.mixin，利用Vue.mixin正确的偷懒

> 刚进入vue项目，有部分细节功能不清楚。段期做了一个vue项目，看到使用vue.mixin功能，就想去了解透怎么应用及使用原理，故有了下文

 **关于Vue.mixin ，官方解释如下**

 混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。 *（不理解，所以看只有看例子咯）*

 Vue.mixin为我们提供了两种混入方式：**局部混入**和**全局混入**； *（好像有关vue都基本上都有局部与合局加载一说，~）*

###  局部混入

 顾名思义就是部分混入，也就是只有引入了mixin的混入对象才可以使用，并且只有在引入了mixin混入对象的组件中才生效； 

 首先自己搭建Vue的开发环境，然后我们在src目录中新建两个vue文件，分别是page1.vue和page2.vue； 

```vue
//page1.vue
<template>
    <div>page1的值是:</div>
</template>

<script>
export default {
  data () {
    return {
     
    }
  },
}
</script>

<style scoped>

</style>
```

```vue
//page2.vue
<template>
    <div>page2的值是:</div>
</template>

<script>
export default {
  data () {
    return {
        
    }
  }
}
</script>

<style scoped>

</style>
```

```vue
//app.vue
<template>
  <div id="app">
    <button @click="method1">page1</button>
    <button @click="method2">page2</button>

    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
  methods:{
    method1(){
      this.$router.push('/page1');
    },
    method2(){
      this.$router.push('/page2');
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

 在src目录下创建router.js文件，配置路由实现跳转 

```javascript
// router.js
import Vue from "vue";
import VueRouter from "vue-router";
Vue.use(VueRouter);

import page1 from "./page1";
import page2 from "./page2";

const routes=[
    {path:"/page1",component:page1},
    {path:"/page2",component:page2}
]


const router=new VueRouter({
    routes
})


export default router
```

 最后将路由引入main.js中： 

```javascript
// main.js
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router.js'

Vue.config.productionTip = false


/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

 完成上述准备工作之后，我们可以看到现在的页面效果如下： 

![vue](https://raw.githubusercontent.com/zhengyuliang/Blog-Back-Up/master/images/vue/vue_mixin.png)

 没有报错，我们开始正式进入学习Vue.mixin： 

*注：终于开始学习了，难得呀*

 首先我们在src目录下新建一个名为mixin的文件夹并在mixin文件中创建一个mixin.js文件： 

```javascript
//抛出混入对象，方便外部访问 mixin.js
export const mixin={
    data(){
        return {
            number:1
        }
    }
}
```

可以看到我们在混入对象中创建了一个变量，是的，混入对象跟Vue实例的格式是一样的；

然后我们可以将mixin.js引入到我们的page1.vue和page2.vue中

```vue
// page1.vue
<template>
    //这里读的值其实是mixin的值，因为这个时候mixin已经混入到vue实例中了
    <div>page1的值是:{{number}}</div>
</template>

<script>
//引入mixin.js
import {mixin} from "./mixin/mixin"
export default {
//这里注意：属性名为mixins，值为数组类型
  mixins:[mixin],
  data () {
    return {
     
    }
  },
}
</script>

<style scoped>

</style>
```

```vue
// page2.vue
<template>
    <div>page2的值是:{{number}}</div>
</template>

<script>
import {mixin} from "./mixin/mixin"
export default {
  mixins:[mixin],
  data () {
    return {
        
    }
  }
}
</script>

<style scoped>

</style>
```

 这个时候我们的混入对象已经成功混入到Vue实例中，你们可以点击看看效果，是可以正常运行并且能读取到值的； 

 现在我们来修改page1.vue的代码： 

```vue
//page1.vue
<template>
    <div>page2的值是:{{number}}</div>
</template>

<script>
import {mixin} from "./mixin/mixin"
export default {
  mixins:[mixin],
  data () {
    return {
        
    }
  }
}
</script>

<style scoped>

</style>
```

page2不变，再运行可以发现，我们的page1.vue中的值是执行了mounted，所以产生了自增

由此，我们可以知道mixin混入对象的变量是不会共享的；也就是你page1发生了变化，并不会通知mixin进行实时刷新数据，发生的变化只会在page1.vue中生效，不影响其他组件；

 现在我们修改mixin.js和page1.vue中的代码： 

```javascript
// mixin.js
export const mixin={
    data(){
        return {
            number:1
        }
    },
    created(){
            console.log("mixin混入对象")
    }
}
```

```vue
// page1.vue
<template>
    <div>page1的值是:{{number}}</div>
</template>

<script>
import {mixin} from "./mixin/mixin"
export default {
  mixins:[mixin],
  data () {
    return {
     
    }
  },
  created(){
          console.log("这里是page1");
  }
}
</script>

<style scoped>

</style>
```

 这个时候我们再运行可以发现控制台输出是这个样子的： 

![vue](https://raw.githubusercontent.com/zhengyuliang/Blog-Back-Up/master/images/vue/vue_mixin_01.png)

是的，mixin混入对象中声明了：如果是同名钩子函数将合并为一个数组，因此都被调用，但是混入对象的钩子将在自身实例钩子之前触发；

值为对象的选项，例如methods,components等如果变量名和mixin混入对象的变量名发生冲突，将会以组件优先并进行递归合并，相当于组件数据直接覆盖了mixin中的同名数据；

 我们可以修改代码mixin.js和page1.vue 

```javascript
// mixin.js
export const mixin={
    data(){
        return {
            number:1
        }
    },
    methods:{
        demo1(){
            console.log("mixin混入对象")
        }
    }
}
```

```vue
//page1.vue
<template>
    <div>page1的值是:{{number}}</div>
</template>

<script>
import {mixin} from "./mixin/mixin"
export default {
  mixins:[mixin],
  data () {
    return {
        number:10
    }
  },
  mounted(){
      this.demo1();
  },
  methods:{
      demo1(){
        console.log("这里是page1");
      }   
  }
}
</script>

<style scoped>

</style>
```

 运行代码我们可以很清晰的看到都是执行我们组件内的值； 

![vue](https://raw.githubusercontent.com/zhengyuliang/Blog-Back-Up/master/images/vue/vue_mixin_02.png)

 因为在vue中我们在实例中声明变量也是通过键值对的形式来声明的，其实也是一个对象； 



####  全局混入

 全局混入我们只需要把mixin.js引入到main.js中，然后将mixin放入到Vue.mixin()方法中即可； 

```javascript
// main.js
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router.js'
import mixin from "./mixin/mixin.js"
Vue.config.productionTip = false
Vue.mixin(mixin)

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

是的，全局混入更为便捷，我们将不用在子组件声明，全局混入将会影响每一个组件的实例，使用的时候需要小心谨慎；这样全局混入之后，我们可以直接在组件中通过this.变量/方法来调用mixin混入对象的变量/方法；



*很多同学可能看到这里会有一些疑问，这不就跟Vuex差不多嘛，其实不是的：*

#### mixin混入对象和Vuex的区别：

**Vuex是状态共享管理，所以Vuex中的所有变量和方法都是可以读取和更改并相互影响的；**

mixin可以定义公用的变量或方法，但是mixin中的数据是不共享的，也就是每个组件中的mixin实例都是不一样的，都是单独存在的个体，不存在相互影响的；

**mixin混入对象值为函数的同名函数选项将会进行递归合并为数组**，两个函数都会执行，只不过先执行mixin中的同名函数；

**mixin混入对象值为对象的同名对象将会进行替换**，都优先执行组件内的同名对象，也就是组件内的同名对象将mixin混入对象的同名对象进行覆盖；