### Array.from() 五个超好用的用途

>  任何一种编程语言都具有超出基本用法的功能，它得益于成功的设计和试图去解决广泛问题。 
>
>  在本文中，我将描述5个有用且有趣的 Array.from() 用例。 

####  **1. 介绍** 

 在开始之前，我们先回想一下 Array.from() 的作用。语法: 

```javascript
Array.from(arrayLike[, mapFunction[, thisArg]])
```

**arrayLike**：必传参数，想要转换成数组的伪数组对象或可迭代对象。

**mapFunction**：可选参数，mapFunction(item，index){...} 是在集合中的每个项目上调用的函数。返回的值将插入到新集合中。

**thisArg**：可选参数，执行回调函数 mapFunction 时 this 对象。这个参数很少使用。

例如，让我们将类数组的每一项乘以2：

```javascript
const someNumbers = { '0': 10, '1': 15, length: 2 };

Array.from(someNumbers, value => value * 2); // => [20, 30]
```

