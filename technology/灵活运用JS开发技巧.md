## [灵活运用JS开发技巧](https://segmentfault.com/a/1190000020749133)

### 前言

何为技巧，意指表现在文学、工艺、体育等方面的巧妙技能。代码作为一门现代高级工艺，推动着人类科学技术的发展，同时犹如文字一样承托着人类文化的进步。

每写好一篇文章，都会使用大量的写作技巧。`烘托、渲染、悬念、铺垫、照应、伏笔、联想、想象、抑扬结合、点面结合、动静结合、叙议结合、情景交融、首尾呼应、衬托对比、白描细描、比喻象征、借古讽今、卒章显志、承上启下、开门见山、动静相衬、虚实相生、实写虚写、托物寓意、咏物抒情`等，这些应该都是我们从小到大写文章而接触到的写作技巧。

作为程序猿的我们，写代码同样也需要大量的写作技巧。一份良好的代码能让人耳目一新，让人容易理解，让人舒服自然，同时也让自己成就感满满(哈哈，这个才是重点)。因此，我整理下三年来自己使用到的一些**JS开发技巧**，希望能让你写出耳目一新、容易理解、舒服自然的代码。

> 以下演示全是ES6版本的书写，在`Webpack`和`Babel`的加持下就不能好好写ES6吗，还写什么ES3和ES5呢，更别管那弱智的IE浏览器了，IE浏览器都快被淘汰了，**Microsoft**都宣布放弃使用自研的浏览器内核而使用**Google**开源的`Chromium`内核了。

### 目录

既然写文章有这么多的写作技巧，那么我也需要对**JS开发技巧**整理一下，起个易记的名字。

- **String Skill**：字符串技巧
- **Number Skill**：数值技巧
- **Boolean Skill**：布尔技巧
- **Array Skill**：数组技巧
- **Object Skill**：对象技巧
- **Function Skill**：函数技巧
- **DOM Skill**：DOM技巧

备注

- 代码只作演示用途，不会详细说明ES6语法
- 如有不明白的语法问题请参考阮一峰老师的[《ES6标准入门》](https://es6.ruanyifeng.com/)
- [《ES6标准入门》](https://es6.ruanyifeng.com/)一直保持更新，建议收藏，平时查看

### String Skill

##### 对比时间

> 时间个位数形式需补0

```javascript
const time1 = "2019-02-14 21:00:00";
const time2 = "2019-05-01 09:00:00";
const overtime = time1 > time2;
// overtime => false
```

##### 格式化金钱

```javascript
const ThousandNum = num => num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
const money = ThousandNum(20190214);
// money => "20,190,214"
```

##### 生成随机ID

```javascript
const RandomId = len => Math.random().toString(36).substr(3, len);
const id = RandomId(10);
// id => "jg7zpgiqva"
```

##### 生成随机HEX色值

```javascript
const RandomColor = () => "#" + Math.floor(Math.random() * 0xffffff).toString(16).padEnd(6, "0");
const color = RandomColor();
// color => "#f03665"
```

##### 生成星级评分

```javascript
const StartScore = rate => "★★★★★☆☆☆☆☆".slice(5 - rate, 10 - rate);
const start = StartScore(3);
// start => "★★★"
```

##### 操作URL查询参数

```javascript
const params = new URLSearchParams(location.search.replace(/\?/ig, "")); // location.search = "?name=young&sex=male"
params.has("young"); // true
params.get("sex"); // "male"
```

### Number Skill

##### 取整

> 代替正数的`Math.floor()`，代替负数的`Math.ceil()`

```javascript
const num1 = ~~ 1.69;
const num2 = 1.69 | 0;
const num3 = 1.69 >> 0;
// num1 num2 num3 => 1 1 1
```

##### 补零

```javascript
const FillZero = (num, len) => num.toString().padStart(len, "0");
const num = FillZero(169, 5);
// num => "00169"
```

##### 转数值

> 只对`null、""、false、数值字符串`有效

```javascript
const num1 = +null;
const num2 = +"";
const num3 = +false;
const num4 = +"169";
// num1 num2 num3 num4 => 0 0 0 169
```

##### 时间戳

```javascript
const timestamp = +new Date("2019-02-14");
// timestamp => 1550102400000
```

##### 精确小数

```javascript
const RoundNum = (num, decimal) => Math.round(num * 10 ** decimal) / 10 ** decimal;
const num = RoundNum(1.69, 1);
// num => 1.7
```

##### 判断奇偶

```javascript
const OddEven = num => !!(num & 1) ? "odd" : "even";
const num = OddEven(2);
// num => "even"
```

##### 取最小最大值

```javascript
const arr = [0, 1, 2];
const min = Math.min(...arr);
const max = Math.max(...arr);
// min max => 0 2
```

##### 生成范围随机数

```javascript
const RandomNum = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
const num = RandomNum(1, 10);
```

### Boolean Skill

##### 短路运算符

```javascript
const a = d && 1; // 满足条件赋值：取假运算，从左到右依次判断，遇到假值返回假值，后面不再执行，否则返回最后一个真值
const b = d || 1; // 默认赋值：取真运算，从左到右依次判断，遇到真值返回真值，后面不再执行，否则返回最后一个假值
const c = !d; // 取假赋值：单个表达式转换为true则返回false，否则返回true
```

##### 判断数据类型

> 可判断类型：undefined、null、string、number、boolean、array、object、symbol、date、regexp、function、asyncfunction、arguments、set、map、weakset、weakmap

```javascript
function DataType(tgt, type) {
    const dataType = Object.prototype.toString.call(tgt).replace(/\[object (\w+)\]/, "$1").toLowerCase();
    return type ? dataType === type : dataType;
}
DataType("young"); // "string"
DataType(20190214); // "number"
DataType(true); // "boolean"
DataType([], "array"); // true
DataType({}, "array"); // false
```

##### 是否为空数组

```javascript
const arr = [];
const flag = Array.isArray(arr) && !arr.length;
// flag => true
```

##### 是否为空对象

```javascript
const obj = {};
const flag = DataType(obj, "object") && !Object.keys(obj).length;
// flag => true
```

##### 满足条件时执行

```javascript
const flagA = true; // 条件A
const flagB = false; // 条件B
(flagA || flagB) && Func(); // 满足A或B时执行
(flagA || !flagB) && Func(); // 满足A或不满足B时执行
flagA && flagB && Func(); // 同时满足A和B时执行
flagA && !flagB && Func(); // 满足A且不满足B时执行
```

##### 为非假值时执行

```javascript
const flag = false; // undefined、null、""、0、false、NaN
!flag && Func();
```

##### 数组不为空时执行

```javascript
const arr = [0, 1, 2];
arr.length && Func();
```

##### 对象不为空时执行

```javascript
const obj = { a: 0, b: 1, c: 2 };
Object.keys(obj).length && Func();
```

##### 函数退出代替条件分支退出

```javascript
if (flag) {
    Func();
    return false;
}
// 换成
if (flag) {
    return Func();
}
```

##### switch/case使用区间

```javascript
const age = 26;
switch (true) {
    case isNaN(age):
        console.log("not a number");
        break;
    case (age < 18):
        console.log("under age");
        break;
    case (age >= 18):
        console.log("adult");
        break;
    default:
        console.log("please set your age");
        break;
}
```

### Array Skill

##### 克隆数组

```javascript
const _arr = [0, 1, 2];
const arr = [..._arr];
// arr => [0, 1, 2]
```

##### 合并数组

```javascript
const arr1 = [0, 1, 2];
const arr2 = [3, 4, 5];
const arr = [...arr1, ...arr2];
// arr => [0, 1, 2, 3, 4, 5];
```

##### 去重数组

```javascript
const arr = [...new Set([0, 1, 1, null, null])];
// arr => [0, 1, null]
```

##### 混淆数组

```javascript
const arr = [0, 1, 2, 3, 4, 5].slice().sort(() => Math.random() - .5);
// arr => [3, 4, 0, 5, 1, 2]
```

##### 清空数组

```javascript
const arr = [0, 1, 2];
arr.length = 0;
// arr => []
```

##### 截断数组

```javascript
const arr = [0, 1, 2];
arr.length = 2;
// arr => [0, 1]
```

##### 交换赋值

```javascript
let a = 0;
let b = 1;
[a, b] = [b, a];
// a b => 1 0
```

##### 过滤空值

> 空值：undefined、null、""、0、false、NaN

```javascript
const arr = [undefined, null, "", 0, false, NaN, 1, 2].filter(Boolean);
// arr => [1, 2]
```

##### 异步累计

```javascript
async function Func(deps) {
    return deps.reduce(async(t, v) => {
        const dep = await t;
        const version = await Todo(v);
        dep[v] = version;
        return dep;
    }, Promise.resolve({}));
}
const result = await Func(); // 需在async包围下使用
```

##### 数组首部插入成员

```javascript
let arr = [1, 2]; // 以下方法任选一种
arr.unshift(0);
arr = [0].concat(arr);
arr = [0, ...arr];
// arr => [0, 1, 2]
```

##### 数组尾部插入成员

```javascript
let arr = [0, 1]; // 以下方法任选一种
arr.push(2);
arr.concat(2);
arr[arr.length] = 2;
arr = [...arr, 2];
// arr => [0, 1, 2]
```

##### 统计数组成员个数

```javascript
const arr = [0, 1, 1, 2, 2, 2];
const count = arr.reduce((t, v) => {
    t[v] = t[v] ? ++t[v] : 1;
    return t;
}, {});
// count => { 0: 1, 1: 2, 2: 3 }
```

##### 解构数组成员嵌套

```javascript
const arr = [0, 1, [2, 3, [4, 5]]];
const [a, b, [c, d, [e, f]]] = arr;
// a b c d e f => 0 1 2 3 4 5
```

##### 解构数组成员别名

```javascript
const arr = [0, 1, 2];
const { 0: a, 1: b, 2: c } = arr;
// a b c => 0 1 2
```

##### 解构数组成员默认值

```javascript
const arr = [0, 1, 2];
const [a, b, c = 3, d = 4] = arr;
// a b c d => 0 1 2 4
```

##### 获取随机数组成员

```javascript
const arr = [0, 1, 2, 3, 4, 5];
const randomItem = arr[Math.floor(Math.random() * arr.length)];
// randomItem => 1
```

##### 创建指定长度数组

```javascript
const arr = [...new Array(3).keys()];
// arr => [0, 1, 2]
```

##### 创建指定长度且值相等的数组

```javascript
const arr = new Array(3).fill(0);
// arr => [0, 0, 0]
```

##### reduce代替map和filter

```javascript
const _arr = [0, 1, 2];

// map
const arr = _arr.map(v => v * 2);
const arr = _arr.reduce((t, v) => {
    t.push(v * 2);
    return t;
}, []);
// arr => [0, 2, 4]

// filter
const arr = _arr.filter(v => v > 0);
const arr = _arr.reduce((t, v) => {
    v > 0 && t.push(v);
    return t;
}, []);
// arr => [1, 2]

// map和filter
const arr = _arr.map(v => v * 2).filter(v => v > 2);
const arr = _arr.reduce((t, v) => {
    v = v * 2;
    v > 2 && t.push(v);
    return t;
}, []);
// arr => [4]
```

### Object Skill

##### 克隆对象

```javascript
const _obj = { a: 0, b: 1, c: 2 }; // 以下方法任选一种
const obj = { ..._obj };
const obj = JSON.parse(JSON.stringify(_obj));
// obj => { a: 0, b: 1, c: 2 }
```

##### 合并对象

```javascript
const obj1 = { a: 0, b: 1, c: 2 };
const obj2 = { c: 3, d: 4, e: 5 };
const obj = { ...obj1, ...obj2 };
// obj => { a: 0, b: 1, c: 3, d: 4, e: 5 }
```

##### 对象字面量

> 获取环境变量时必用此方法，用它一直爽，一直用它一直爽

```javascript
const env = "prod";
const link = {
    dev: "Development Address",
    test: "Testing Address",
    prod: "Production Address"
}[env];
// link => "Production Address"
```

##### 对象变量属性

```javascript
const flag = false;
const obj = {
    a: 0,
    b: 1,
    [flag ? "c" : "d"]: 2
};
// obj => { a: 0, b: 1, d: 2 }
```

##### 创建纯空对象

```javascript
const obj = Object.create(null);
Object.prototype.a = 0;
// obj => {}
```

##### 删除对象无用属性

```javascript
const obj = { a: 0, b: 1, c: 2 }; // 只想拿b和c
const { a, ...rest } = obj;
// rest => { b: 1, c: 2 }
```

##### 解构对象属性嵌套

```javascript
const obj = { a: 0, b: 1, c: { d: 2, e: 3 } };
const { c: { d, e } } = obj;
// d e => 2 3
```

##### 解构对象属性别名

```javascript
const obj = { a: 0, b: 1, c: 2 };
const { a, b: d, c: e } = obj;
// a d e => 0 1 2
```

##### 解构对象属性默认值

```javascript
const obj = { a: 0, b: 1, c: 2 };
const { a, b = 2, d = 3 } = obj;
// a b d => 0 1 3
```

### Function Skill

##### 函数自执行

```javascript
const Func = function() {}(); // 常用

(function() {})(); // 常用
(function() {}()); // 常用
[function() {}()];

+ function() {}();
- function() {}();
~ function() {}();
! function() {}();

new function() {};
new function() {}();
void function() {}();
typeof function() {}();
delete function() {}();

1, function() {}();
1 ^ function() {}();
1 > function() {}();
```

##### 隐式返回值

> 只能用于`单语句返回值箭头函数`，如果返回值是对象必须使用`()`包住

```javascript
const Func = function(name) {
    return "I Love " + name;
};
// 换成
const Func = name => "I Love " + name;
```

##### 一次性函数

> 适用于运行一些只需执行一次的初始化代码

```javascript
function Func() {
    console.log("x");
    Func = function() {
        console.log("y");
    }
}
```

##### 惰性载入函数

> 函数内判断分支较多较复杂时可大大节约资源开销

```javascript
function Func() {
    if (a === b) {
        console.log("x");
    } else {
        console.log("y");
    }
}
// 换成
function Func() {
    if (a === b) {
        Func = function() {
            console.log("x");
        }
    } else {
        Func = function() {
            console.log("y");
        }
    }
    return Func();
}
```

##### 检测非空参数

```javascript
function IsRequired() {
    throw new Error("param is required");
}
function Func(name = IsRequired()) {
    console.log("I Love " + name);
}
Func(); // "param is required"
Func("You"); // "I Love You"
```

##### 字符串创建函数

```javascript
const Func = new Function("name", "console.log(\"I Love \" + name)");
```

##### 优雅处理错误信息

```javascript
try {
    Func();
} catch (e) {
    location.href = "https://stackoverflow.com/search?q=[js]+" + e.message;
}
```

##### 优雅处理Async/Await参数

```javascript
function AsyncTo(promise) {
    return promise.then(data => [null, data]).catch(err => [err]);
}
const [err, res] = await AsyncTo(Func());
```

##### 优雅处理多个函数返回值

```javascript
function Func() {
    return Promise.all([
        fetch("/user"),
        fetch("/comment")
    ]);
}
const [user, comment] = await Func(); // 需在async包围下使用
```

### DOM Skill

##### 显示全部DOM边框

> 调试页面元素边界时使用

```javascript
[].forEach.call($$("*"), dom => {
    dom.style.outline = "1px solid #" + (~~(Math.random() * (1 << 24))).toString(16);
});
```

##### 自适应页面

> 页面基于一张设计图但需做多款机型自适应，元素尺寸使用`rem`进行设置

```javascript
function AutoResponse(width = 750) {
    const target = document.documentElement;
    target.clientWidth >= 600
        ? (target.style.fontSize = "80px")
        : (target.style.fontSize = target.clientWidth / width * 100 + "px");
}
```

##### 过滤XSS

```javascript
function FilterXss(content) {
    let elem = document.createElement("div");
    elem.innerText = content;
    const result = elem.innerHTML;
    elem = null;
    return result;
}
```

##### 存取LocalStorage

> 反序列化取，序列化存

```javascript
const love = JSON.parse(localStorage.getItem("love"));
localStorage.setItem("love", JSON.stringify("I Love You"));
```

### 总结

写到最后总结得差不多了，如果后续我想起还有哪些遗漏的**JS开发技巧**，会继续在这篇文章上补全。

最后送大家一个键盘！

```javascript
(_=>[..."`1234567890-=~~QWERTYUIOP[]\\~ASDFGHJKL;'~~ZXCVBNM,./~"].map(x=>(o+=`/${b='_'.repeat(w=x<y?2:' 667699'[x=["Bs","Tab","Caps","Enter"][p++]||'Shift',p])}\\|`,m+=y+(x+'    ').slice(0,w)+y+y,n+=y+b+y+y,l+=' __'+b)[73]&&(k.push(l,m,n,o),l='',m=n=o=y),m=n=o=y='|',p=l=k=[])&&k.join`
`)()
```