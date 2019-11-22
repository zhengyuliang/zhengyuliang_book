### Vue.set()和this.$set()应用的场景

>  背景：在vue中改变对象的属性值或者通过索引改变数组时，对应修改的字段不会重新在页面渲染。 

 例如： 

```javascript
const vueInstance = new Vue({
  data: {
    arr: [1, 2],
    obj1: {
        a: 3
    }
  }
});

vueInstance.$data.arr[0] = 3;  // 页面不会重新渲染
vueInstance.$data.obj1.b = 3;  // 页面不会重新渲染
```

 经过查看官方文档发现这部分说明如下： 

> **Vue.set()**向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发视图更新。
>
> 它必须用于向响应式对象上添加新属性，因为 **Vue** 无法探测普通的新增属性 (比如 this.myObject.newProperty = 'hi')

 所以按照官方的写法应该是： 

```javascript
Vue.set(vueInstance.$data.arr, 0, 3);  // 这样操作数组可以让页面重新渲染
vueInstance.$set(vueInstance.$data.arr, 0, 3); // 这样操作数组也可以让页面重新渲染
Vue.set(vueInstance.$data.obj1, b, 3);  // 这样操作对象可以让页面重新渲染
vueInstance.$set(vueInstance.$data.obj1, b, 3); // 这样操作对象也可以让页面重新渲染
```

#### Vue.set()和this.$set()实现原理

 是时候看一波这两个api的源码了，我们先来看看Vue.set()的源码： 

```javascript
import { set } from '../observer/index'

...
Vue.set = set
...
```

 再来看看this.$set()的源码： 

```javascript
import { set } from '../observer/index'

...
Vue.prototype.$set = set
...
```

发现这两个api的实现原理基本一模一样，都是使用了set函数。

set函数是从 ../observer/index 文件中导出的，区别在于Vue.set()是将set函数绑定在Vue构造函数上，this.$set()是将set函数绑定在Vue原型上。

#### 数组的实现原理

 继续源码： 

```javascript
if (Array.isArray(target) && isValidArrayIndex(key)) {
    target.length = Math.max(target.length, key)
    target.splice(key, 1, val)
    return val
  }
```

首先if判断当前target是不是数组，并且key的值是有效的数组索引。

然后将target数组的长度设置为target.length和key中的最大值，这里为什么要这样做呢?是因为我们可能会进行下面这种骚操作：

```javascript
arr1 = [1,3];
Vue.set(arr1,10,1)  // 如果不那样做，这种情况就会出问题
```

接着向下看，我们发现这里直接调用了target.splice(key, 1, val)，在前面我们说过调用arrayMethods提供的**push**、**pop**等7个方法可以导致页面重新渲染，刚好splice也是属性arrayMethods提供的7个方法中的一种。

 **总结一下Vue.set数组实现的原理：
其实Vue.set()对于数组的处理其实就是调用了splice方法，是不是发现其实很简单~~** 



#### 对象的实现原理

 续看源码： 

```javascript
if (key in target && !(key in Object.prototype)) {
    target[key] = val
    return val
  }
```

这里先判断如果key本来就是对象中的一个属性，并且key不是Object原型上的属性。说明这个key本来就在对象上面已经定义过了的，直接修改值就可以了，可以自动触发响应。
 注意：对于**新增的对象属性不会重新渲染**，这也刚好解释了我们使用 vueInstance.$data.obj1.b = 3; 的时候为什么页面不会重新渲染，因为这里的属性b不是对象的已有属性，也就是说属性b没有进行过依赖收集，所以才会导致修改属性b的值页面不会重新渲染。

 继续看源码： 

```javascript
defineReactive(ob.value, key, val)
  ob.dep.notify()
  return val
```

这里其实才是vue.set()真正处理对象的地方。`defineReactive(ob.value, key, val)`的意思是给新加的属性添加依赖，以后再直接修改这个新的属性的时候就会触发页面渲染。
 `ob.dep.notify()`这句代码的意思是触发当前的依赖（这里的依赖依然可以理解成渲染函数），所以页面就会进行重新渲染。