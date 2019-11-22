### 异步神器Async-await介绍与填坑

> 神，请好好研究一下async-await功能，接下来的业务需求中，用处巨巨巨巨大，请同我学习，我教你们。



#### async - 定义异步函数

 `async` 译：异步，是 Generator 函数的语法糖。该函数会返回一个promise对象，可以使用`then`方法添加回调函数，如果在函数内直接 `return`，Async会通过**Promise.resolve()**将其封装成Promise()对象，也可以通过`.then`添加回调函数

```javascript
async function pms() {
  return 'abc'
}
console.log(pms())  // [object Promise] { ... }
```

  以上方法执行返回了一个promise对象，其执行等效于 

```javascript
async function pms() {
  return new Promise((resolve, reject) => {
    resolve('abc')
  })
}
```

 在没有配合`await`时，我们可以调用promise的`.then`方法得到执行结果。当然也会有`.catch`方法 

```javascript
pms().then(res => {
  console.log(res)  // 'abc'
}).catch(e => {
  console.log('错误')
})
```

#### await - 暂停异步函数的执行

> 您历害了，你历害了，没有你怎么处理，await不错

`await`，即：async wait，旨在**等待异步执行结束，使用于async函数内部**。异步未执行结束，阻塞当前代码。嗯哼？**阻塞！！！**

  与线程阻塞不同的是，`await` 的阻塞发生在 `async` 函数内部，可以理解为一个异步的阻塞。跟在`await`后的JS表达式，可以等待很多类型的事件，但初衷是用于等待Promise对象。如果`await`的对象是promise对象，则阻塞异步函数代码执行等待promise的`resolve`状态，如果是同步执行的表达式则立即执行。

#### async/await的使用

```javascript
function asyncFunc(time) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {  // 用setTimeout模拟异步
      resolve('async result')
    }, time)
  })
}

function normalFunc() {
  console.log('normal result')
}

async function awaitDemo() {
  await normalFunc()  // 执行立即打印 normal result
  const res = await asyncFunc(1000)
  console.log(res) // 执行1s后打印 ‘async result’
}

awaitDemo() // 执行 
```

 以上的例子比较常规，再看看下面这个例子 

```javascript
async function func() {
  console.log('123')
}
async function run() {
  const res = await func()
  console.log(res)  // 123
}
run()
```

该例子中，`func`看似一个普通函数，但经`async`定义后，会返回一个**promise**对象，此时的`await`等待的就是该**promise**对象的`resolve`参数，即`123`。
  对比上一个例子中的`normalFunc`，主要有两个理解点
 （1）增加了**async**的普通函数变成了一个异步函数，**await**等待的对象为**promise**对象，返回**resolve**参数
 （2）**await**可以等待任何东西，如果等待的是普通内容，则直接返回该内容



#### 【填坑1】异常处理

> 请用**try-catch**处理异常内容

 细心的你应该已经发现，`await` 返回的是promise的`resolve`参数，但对于`catch`却没有实际的处理。那当我们的请求发生异常时，该怎么办？答案是使用`try-catch`，在Promise中的`.catch()`分支会流入`catch`中

```javascript
function asyncFunc(time) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {  // 用setTimeout模拟异步
      reject('promise reject')
    }, time)
  })
}
// 为了处理Promise.reject 的情况我们应该将代码块用 try catch 捕获一下
async function awaitDemo() {
  try {
    const res = await asyncFunc(1000)
    console.log(res)
  } catch (err) {
    console.log(err.message || 'Uncatch Error')
  }
}

awaitDemo()  // 如果没有使用try-catch，执行会报错
```



#### 【填坑2】并行请求

 对于一个真实的业务需求，通常会有多个异步请求需要同时执行，如获取左侧目录树，和登录账户的用户信息等。因await遇到异步会阻塞，当一个async函数内有多个异步函数需要调用时，就会出现相互等待的现象。天哪，**“异步” 变成了 “同步”了**，这不是我们所希望的

```javascript
function asyncFunc() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('request done')
    })
  })
}
async function bugDemo() {
  await asyncFunc()
  await asyncFunc()
  await asyncFunc()
  console.log('finish')
}
```

> 历害了，可能用paomise.all() 把异步变成同步

以上代码正常执行了，没有报出异常，但仔细观察控制台 timeline 可以发现，三个函数是同步顺序执行的，其罪魁祸首就是`await`的等待机制。那怎么解决这个问题？

  其实`async-await` 只是promise的语法糖，但其并不能代替Promise，从对异常的处理上就可以看出，还需要引入`try-catch` ，本问题是另一个体现。解决办法需要使用promise的**`.all()`**

>  `promise.all( iterable )`，当所有请求都为resolve时，返回`resolve`状态 

```javascript
async function correctDemo() {
  const f1 = asyncFunc()
  const f2 = asyncFunc()
  const f3 = asyncFunc()
  await Promise.all([f1, f2, f3]);
  console.log('finish')
}
```

#### 【填坑3】await必须在async的上下文中

 `await` 并不只是使用在`async` 函数中即可，还必须在`asyn`函数的上下文中 

```javascript
// 虽在async函数里使用，但在forEach上下文中，异常
async function errorDemo() {
    [1, 2, 3, 4, 5].forEach(item => {
        await item;
    });
}
errorDemo() // SyntaxError: await is only valid in async function
// await 必须在async函数的上下文中
async function correctDemo() {
    let arr = [1, 2, 3, 4, 5];
    for (let i = 0; i < arr.length; i ++) {
        await arr[i];
    }
}
```

