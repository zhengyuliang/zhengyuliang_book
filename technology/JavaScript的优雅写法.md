### 探讨JavaScript的优雅写法

>  用最合适的代码执行正确的逻辑， 句话意思是说，每一行代码要做到简洁，容易理解 

「优雅」最直观的表现就是**看起来舒服**，要做到这一点不难，那就是代码书写讲究一个规范，可以是你最习惯的换行，最习惯的变量命名等等。不要一个几百行代码，书写习惯换了好几次。做到这一点，那么自己看起来就能舒服了。但是这仅仅是最基本的，公司中的项目往往是不止一个人开发，更多的是协同开发。代码仅仅是自己看起来舒服还不行，还要做到其他人**看起来也舒服**。要做到这一点，就需要**把每个人书写的习惯统一起来，形成团队规范**。

> 为了更好的写代码，提升自己，我忍了。看的文章越多，发现自己懂的越少。唉，为什么？求解！

> 优雅写法，以前对于我来说是没有看回事的。写代码就是把整个模块切分为不同的模块，让每个模块更好的工作，并统一起来。看到这文章，也学学。

 **判断对内的「优雅」很简单，自己看代码能够毫不费劲就能理解逻辑，并且没有「乱糟糟」，「多余」的感觉。** 

#### 具体的优雅写法实例

1. 运算符处换行时，运算符在新行的行首。
   在进行复杂的条件判断时，我们会使用到&&与||，这个使用过长的判断条件很容易让人心生厌恶，这个时候以一种漂亮的书写风格会更人让我们接受。 

   ```javascript
   // good
   if (user.isAuthenticated()
       && user.isInRole('admin')
       && user.hasAuthority('add-admin')
       || user.hasAuthority('delete-admin')
   ) {
       // Code
   }
   var result = number1 + number2 + number3
       + number4 + number5;
   
   // bad
   if (user.isAuthenticated() &&
       user.isInRole('admin') &&
       user.hasAuthority('add-admin') ||
       user.hasAuthority('delete-admin')) {
       // Code
   }
   
   var result = number1 + number2 + number3 +
       number4 + number5;
   ```

   

2.  boolean 类型的变量使用 is 或 has 开头
   这样做的好处是能一眼看出该变量就是一个布尔变量。 

   ```javascript
   var isReady = false;
   var hasMoreCommands = false;
   ```

3.  尽可能使用简洁表达式 

    在讲这个之前，先来复习一下JS中的数据类型及判断方式。 

   ![javascript](https://github.com/zhengyuliang/Blog-Back-Up/blob/master/images/javascript/javascript_01.png?raw=true)

   如上图所示，JS中的数据类型分为**基本数据类型**和**引用数据类型**，**引用数据类型**一般也被直接叫做**对象**。对象可以看做是一个复合数据类型，我们可以用它来组装其他数据类型。其中被官方组装好的也是用的很广泛的主要是四种：**Array(数组)**，**Function(函数)**，**RegExp(正则)**和**Date(日期)**。

   ![类型判断](https://github.com/zhengyuliang/Blog-Back-Up/blob/master/images/javascript/javascript_02.png?raw=true)

    那么在使用 if else时，对于条件的判断，**我们尽可能使用简洁表达式**： 

   ```javascript
   // 字符串为空
   // good
   if (!name) {
       // ......
   }
   // bad
   if (name === '') {
       // ......
   }
   
   // 字符串非空
   // good
   if (name) {
       // ......
   }
   
   // bad
   if (name !== '') {
       // ......
   }
   
   // 数组非空
   // good
   if (collection.length) {
       // ......
   }
   
   // bad
   if (collection.length > 0) {
       // ......
   }
   
   // 布尔不成立
   // good
   if (!notTrue) {
       // ......
   }
   
   // bad
   if (notTrue === false) {
       // ......
   }
   
   // null 或 undefined
   // good
   if (noValue == null) {
     // ......
   }
   
   // bad
   if (noValue === null || typeof noValue === 'undefined') {
     // ......
   }
   ```

4.   如果函数或全局中的 else 块后没有任何语句，可以**删除 else**。 

   > 这个用的多，不用说，了解

   ```javascript
   // good
   function getName() {
       if (name) {
           return name;
       }
   
       return 'unnamed';
   }
   
   // bad
   function getName() {
       if (name) {
           return name;
       }
       else {
           return 'unnamed';
       }
   }
   ```

5.  不要在循环体中包含函数表达式，事先将函数提取到循环体外 

   ```javascript
   // good
   function clicker() {
       // ......
   }
   
   for (var i = 0, len = elements.length; i < len; i++) {
       var element = elements[i];
       addListener(element, 'click', clicker);
   }
   
   // bad
   for (var i = 0, len = elements.length; i < len; i++) {
       var element = elements[i];
       addListener(element, 'click', function () {});
   }
   ```

6.  对循环内多次使用的不变值，在循环外用变量缓存 

   ```javascript
   // good
   var width = wrap.offsetWidth + 'px';
   for (var i = 0, len = elements.length; i < len; i++) {
       var element = elements[i];
       element.style.width = width;
       // ......
   }
   
   // bad
   for (var i = 0, len = elements.length; i < len; i++) {
       var element = elements[i];
       element.style.width = wrap.offsetWidth + 'px';
       // ......
   }
   ```

7.  对有序集合进行顺序无关的遍历时，使用逆序遍历 

   >  解释:逆序遍历可以节省变量，代码比较优化。 

   ```javascript
     var len = elements.length;
     while (len--) {
         var element = elements[len];
         // ......
         }
   ```

8.  类型检测优先使用 **typeof**。对象类型检测使用 **instanceof**。**null** 或 **undefined** 的检 测使用 == null 

9.  转换成 number 时，通常使用+ 

   ```javascript
   // good 
   +str;
   
   //bad 
   Number(str);
   ```

10.  转换成 **boolean** 时，使用!! 

    ```javascript
    var num = 3.14;
    !!num;
    ```

11.  属性访问尽量使用点语法 

    >  解释:如非必须使用[]的情况，则建议优先使用点语法。通常在 JavaScript 中声明的对象，属性
    > 命名是使用 Camel 命名法，用 . 来访问更清晰简洁。 

    ```javascript
    info.age;
    info['more-info'];
    ```

12.  清空数组使用 .length=0 

13.  使用 let 和 const 定义变量，尽量不使用 **var** 

    >  解释:使用 **let** 和 **const** 定义时，变量作用域范围更明确。 

14.  使用 Object.keys 或 Object.entries 进行对象遍历 

    >  解释:不建议使用 for .. in 进行对象的遍历，以避免遗漏 hasOwnProperty 产生的错误。 

15.  使用&&或||代替三元表达式 

    >  逻辑与运算和逻辑或运算，实际上可以看做是条件运算的语法糖。同时JS
    > 中的三元运算符，也可以看做是条件运算的简写方式。我们可以使用&&和||替代，这样减少了代码量，还便于理解。 

    ```javascript
    // 不推荐
    let userName = localStorage.userName ? localStorage.userName : 'admin'
    // 推荐
    let userName = localStorage.userName || 'admin'
    ```

    

### JavaScript 复杂判断的优雅写法

> 前言: 之前项目中看到很多人代码里用了很多if-else语法，甚至达到了痴迷的地步，大大影响了代码的可读性和简洁度。正好最近看到网上一篇很好的对JavaScript中复杂判断的分析文章，这里整理一下分享给大家，希望能对各位有所帮助。

> 我就是一直用**if-else**，大哭，非常大哭。看来要经常研究怎么处理代码咯

#### if else 举例(一元判断)

```javascript
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1 开团进行中 2 开团失败 3 商品售罄 4 开团成功 5 系统取消
 */
const onButtonClick = (status)=>{
  if(status == 1){
    sendLog('processing')
    jumpTo('IndexPage')
  }else if(status == 2){
    sendLog('fail')
    jumpTo('FailPage')
  }else if(status == 3){
    sendLog('fail')
    jumpTo('FailPage')
  }else if(status == 4){
    sendLog('success')
    jumpTo('SuccessPage')
  }else if(status == 5){
    sendLog('cancel')
    jumpTo('CancelPage')
  }else {
    sendLog('other')
    jumpTo('Index')
  }
}
```

 通过代码可以看到这个按钮的点击逻辑：根据不同status做两件事情，sendLog和跳转到对应页面，大家可以很轻易的提出这段代码的改写方案，switch出场： 

> 现在本人用的比较多的就是switch，发现还比较好用，哈哈哈哈

####  **switch代替** 

```javascript
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1 开团进行中 2 开团失败 3 商品售罄 4 开团成功 5 系统取消
 */
const onButtonClick = (status)=>{
  switch (status){
    case 1:
      sendLog('processing')
      jumpTo('IndexPage')
      break
    case 2:
    case 3:
      sendLog('fail')
      jumpTo('FailPage')
      break  
    case 4:
      sendLog('success')
      jumpTo('SuccessPage')
      break
    case 5:
      sendLog('cancel')
      jumpTo('CancelPage')
      break
    default:
      sendLog('other')
      jumpTo('Index')
      break
  }
}
```

> 听说还有更简单的写法，用的比较少，来来，我们一起看看

####  **对象模式简写** 

```javascript
const actions = {
  '1': ['processing','IndexPage'],
  '2': ['fail','FailPage'],
  '3': ['fail','FailPage'],
  '4': ['success','SuccessPage'],
  '5': ['cancel','CancelPage'],
  'default': ['other','Index'],
}
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1开团进行中 2开团失败 3 商品售罄 4 开团成功 5 系统取消
 */
const onButtonClick = (status)=>{
  let action = actions[status] || actions['default'],
      logName = action[0],
      pageName = action[1]
  sendLog(logName)
  jumpTo(pageName)
}
```

上面代码确实看起来更清爽了，这种方法的聪明之处在于：将判断条件作为对象的属性名，将处理逻辑作为对象的属性值，在按钮点击的时候，通过对象属性查找的方式来进行逻辑判断，这种写法特别适合一元条件判断的情况。还有es6里面的Map对象简写:

> 下面就是map，这个用的比较多，不过正常都不是这样用，涨知识了
>
> new Map() 这个功能要好好学学，理解一下

####  **Map** 

```javascript
const actions = new Map([
  [1, ['processing','IndexPage']],
  [2, ['fail','FailPage']],
  [3, ['fail','FailPage']],
  [4, ['success','SuccessPage']],
  [5, ['cancel','CancelPage']],
  ['default', ['other','Index']]
])
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1 开团进行中 2 开团失败 3 商品售罄 4 开团成功 5 系统取消
 */
const onButtonClick = (status)=>{
  let action = actions.get(status) || actions.get('default')
  sendLog(action[0])
  jumpTo(action[1])
}
```

这样写用到了es6里的Map对象，是不是更爽了？Map对象和Object对象有什么区别呢？

1.一个对象通常都有自己的原型，所以一个对象总有一个"prototype"键。
 2.一个对象的键只能是字符串或者Symbols，但一个Map的键可以是任意值。
 3.你可以通过size属性很容易地得到一个Map的键值对个数，而对象的键值对个数只能手动确认。

我们需要把问题升级一下，以前按钮点击时候只需要判断status，现在还需要判断用户的身份：

#### if else 举例(二元判断)

```javascript
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1开团进行中 2开团失败 3 开团成功 4 商品售罄 5 有库存未开团
 * @param {string} identity 身份标识：guest客态 master主态
 */
const onButtonClick = (status,identity)=>{
  if(identity == 'guest'){
    if(status == 1){
      //do sth
    }else if(status == 2){
      //do sth
    }else if(status == 3){
      //do sth
    }else if(status == 4){
      //do sth
    }else if(status == 5){
      //do sth
    }else {
      //do sth
    }
  }else if(identity == 'master') {
    if(status == 1){
      //do sth
    }else if(status == 2){
      //do sth
    }else if(status == 3){
      //do sth
    }else if(status == 4){
      //do sth
    }else if(status == 5){
      //do sth
    }else {
      //do sth
    }
  }
}
```

 从上面的例子我们可以看到，当你的逻辑升级为二元判断时，你的判断量会加倍，你的代码量也会加倍，这时怎么写更清爽呢？ 

> 我能说很不爽吧，救新解！！！！

####  **Map实现** 

```javascript
const actions = new Map([
  ['guest_1', ()=>{/*do sth*/}],
  ['guest_2', ()=>{/*do sth*/}],
  ['guest_3', ()=>{/*do sth*/}],
  ['guest_4', ()=>{/*do sth*/}],
  ['guest_5', ()=>{/*do sth*/}],
  ['master_1', ()=>{/*do sth*/}],
  ['master_2', ()=>{/*do sth*/}],
  ['master_3', ()=>{/*do sth*/}],
  ['master_4', ()=>{/*do sth*/}],
  ['master_5', ()=>{/*do sth*/}],
  ['default', ()=>{/*do sth*/}],
])

/**
 * 按钮点击事件
 * @param {string} identity 身份标识：guest客态 master主态
 * @param {number} status 活动状态：1 开团进行中 2 开团失败 3 开团成功 4 商品售罄 5 有库存未开团
 */
const onButtonClick = (identity,status)=>{
  let action = actions.get(`${identity}_${status}`) || actions.get('default')
  action.call(this)
}
```

 上述代码核心逻辑是：把两个条件拼接成字符串，并通过以条件拼接字符串作为键，以处理函数作为值的Map对象进行查找并执行，这种写法在多元条件判断时候尤其好用。 

 当然上述代码如果用Object对象来实现也是类似的： 

####  **Object实现** 

```javascript
const actions = {
  'guest_1':()=>{/*do sth*/},
  'guest_2':()=>{/*do sth*/},
  //....
}

const onButtonClick = (identity,status)=>{
  let action = actions[`${identity}_${status}`] || actions['default']
  action.call(this)
}
```

 如果有些同学觉得把查询条件拼成字符串有点别扭，那还有一种方案，就是用Map对象，以Object对象作为key： 



