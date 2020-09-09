## JavaScript数组求并集，交集和差集

**现有两数组a = [1, 2, 3]，b = [2, 4, 5]，求a，b数组的并集，交集和差集。**

#####  **1、ES7方法** 

 ES7新增了一个Array.prototype.includes的数组方法，用于返回一个数组是否包含指定元素，结合filter方法。 

```javascript
  // 并集
let union = a.concat(b.filter(v => !a.includes(v))) // [1,2,3,4,5]
// 交集
let intersection = a.filter(v => b.includes(v)) // [2]
// 差集
let difference = a.concat(b).filter(v => a.includes(v) && !b.includes(v)) // [1,3]
```

##### 2、ES6方法

 ES6中新增的一个Array.from方法，用于将类数组对象和可遍历对象转化为数组。只要类数组有length长度，基本都可以转化为数组。结合Set结构实现数学集求解。 

```javascript
  	let a = [1, 2, 3]
    console.log(a)
 
    let b = [2, 4, 5]
 
    let aSet = new Set(a)
    let bSet = new Set(b)
    
    // 并集
    let union = Array.from(new Set(a.concat(b))) // [1,2,3,4,5]
    console.log(union)
    
    // 交集
    let intersection = Array.from(new Set(a.filter(v => bSet.has(v)))// [2]
    )
    
    // 差集
    let differenceNew = Array.from(new Set(a.concat(b).filter(v => aSet.has(v) && !bSet.has(v))) [1,3]
    )
    console.log(differenceNew)
```

#####  **ES5方法** 

 ES5可以利用filter和indexOf进行数学集操作，但是，由于indexOf方法中NaN永远返回-1，所以需要进行兼容处理。 

 01不考虑**NaN**(数组中不含**NaN**) 

```javascript
 	var a = [1,2,3];
    var b = [2,4,5];
 
    // 并集
    var union = a.concat(b.filter(function(v) {
        return a.indexOf(v) === -1})) // [1,2,3,4,5]
    // 交集
    var intersection = a.filter(function(v){ return b.indexOf(v) > -1 }) // [2]
 
    // 差集
    var difference = a.filter(function(v){ return b.indexOf(v) === -1 })// [1,3]
 
    console.log(union)
    console.log(intersection)
    console.log(difference)
```

 02考虑**NaN** 

```javascript
  	var a = [1, 2, 3, NaN];
    var b = [2, 4, 5];
 
    var aHasNaN = a.some(function (v) {
        return isNaN(v)
    })
    var bHasNaN = b.some(function (v) {
        return isNaN(v)
    })
 
    // 并集
    var union = a.concat(b.filter(function (v) {
        return a.indexOf(v) === -1 && !isNaN(v)
    })).concat(!aHasNaN & bHasNaN ? [NaN] : []) // [1,2,3,4,5,NaN]
 
    // 交集
    var intersection = a.filter(function (v) {
        return b.indexOf(v) > -1
    }).concat(aHasNaN & bHasNaN ? [NaN] : []) // [2]
 
    // 差集
    var difference = a.filter(function (v) {
        return b.indexOf(v) === -1 && !isNaN(v)
    }).concat(aHasNaN && !bHasNaN ? [NaN] : [])//1,3,NaN
 
    console.log(union)
    console.log(intersection)
    console.log(difference)

```

### filter完成数组去重

```javascript
 var r;
var arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];

r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});

alert(r);
// 去除重复元素依靠的是indexOf总是返回第一个元素的位置，后续的重复元素位置与indexOf返回的位置不相等，因此被filter滤掉了。
```

### 数组对象的差集、并集等

> 数组的对像的差集与并集的判断等功能

```javascript
// 差集
// arr arr1 数组对象的对比 type属性
export function chackAontrastArr (arr, arr1, type) {
  return [...arr].filter(x => [...arr1].every(y => y[type] !== x[type]))
}

// 并集
// arr arr1 数组对象的对比 type属性
export function someAontrastArr (arr, arr1) {
  return [...arr1].filter(x => [...arr].some(y => y[type] === X[type]))
}
```

