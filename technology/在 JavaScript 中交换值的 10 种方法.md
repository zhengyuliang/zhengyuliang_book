## [在 JavaScript 中交换值的 10 种方法](https://segmentfault.com/a/1190000023824691)

在开发过程中又是我们需要对值进行交换。一般我们都在用一种简单的解决方案：“临时变量”。不过还有更好的办法，而且不只有一个，有很多。有时我们在网上搜寻解决方案，找到后复制粘贴，但是从没想过这小段代码是怎样工作的。现在我们该学习一下应该怎样轻松高效地交换值了。

## 1 使用临时变量

先是最简单的一种。

```javascript
function swapWithTemp(num1,num2){
  console.log(num1,num2)

  var temp = num1;
  num1 = num2;
  num2 = temp;

  console.log(num1,num2)
}

swapWithTemp(2.34,3.45)
```

## 2 使用算术运算符 + 和 -

还可以用一些数学魔术来交换值。

```javascript
function swapWithPlusMinus(num1,num2){
  console.log(num1,num2)

  num1 = num1+num2;
  num2 = num1-num2;
  num1 = num1-num2;

  console.log(num1,num2)
}

swapWithPlusMinus(2.34,3.45)
```

让我们来看看它是如何工作的。我们在第 4 行获得两个数字的总和。现在，如果从和中减去一个数字，那么另一个数字就正确了。这就是第 5 行所做的工作。从存储在 `num1` 变量中的总和中减去 `num2` 会得到存储在 `num2` 中的原始 `num1` 值。同样，在第 6 行的 `num1` 中得到 `num2` 的值。

**小心**：还有一个与 `+` 和 `-` 互换的单行代码方案，不过。。。

它是这样的：

```javascript
function swapWithPlusMinusShort(num1,num2){
  console.log(num1,num2)

  num2 = num1+(num1=num2)-num2;

  console.log(num1,num2)
}

swapWithPlusMinusShort(2,3)
```

上面的代码给出了预期的结果。 `()` 中的表达式将 `num2` 存储在 `num1` 中，然后减去 `num1 - num2`，除了减去 `num2 - num2 = 0` 之外什么也没有做，因此得到了结果。但是当使用浮点数时，会看到一些意外的结果。

试着执行下面的代码并查看结果：

```javascript
function swapWithPlusMinusShort(num1,num2){
  console.log(num1,num2)

  num2 = num1+(num1=num2)-num2;

  console.log(num1,num2)
}

swapWithPlusMinusShort(2,3.1)
```

## 3 仅使用 + 或 - 运算符

仅通过使用 `+` 运算符就可以达到同时使用 `+` 和 `-` 相同的结果。

看下面的代码：

```javascript
function swapWithPlus(num1,num2){
  console.log(num1,num2)

  num2 = num1 + (num1=num2, 0)

  console.log(num1,num2)
}

//Try with - operator
swapWithPlus(2.3,3.4)
```

上面的代码是有效的，但牺牲了可读性。在第 4 行的 `()` 中，我们将 `num1` 赋值给 `num2`，而旁边的 `0` 是返回值。简而言之，第 4 行的运算逻辑如下所示：

```javascript
num2 = num1 + 0 
=> num2 = num1.
```

所以得到了正确结果。

**注意**：一些 JavaScript 引擎可能会对上面的代码进行优化，从而忽略 `+ 0`。

## 4 使用算术运算符 * 和 /

让我们用 `*` 和`/` 运算符玩更多的花样。

其原理与先前的方法相同，但是有一些小问题。

```javascript
function swapWithMulDiv(num1,num2){
  console.log(num1,num2)

  num1 = num1*num2;
  num2 = num1/num2;
  num1 = num1/num2;

  console.log(num1,num2)
}

swapWithMulDiv(2.34,3.45)
```

与上一个方法相同。首先得到两个数字的乘积，并将它们存储在 `num1` 中。然后在第 5 行，把 `num2` 与这个结果相除，得到第一个数字，然后重复此过程以获得第二个数字。

现在你成“ **数学家**” 了。

不过那小问题在哪儿呢？

让我们来尝试一下：

```javascript
function swapWithMulDiv(num1,num2){
  console.log(num1,num2)

  num1 = num1*num2;
  num2 = num1/num2;
  num1 = num1/num2;

  console.log(num1,num2)
}

//试着改变数字的值，看看会发生什么
swapWithMulDiv(2.34,0)
```

我们的值没有交换，而是得到了一个奇怪的 `NaN`，这是怎么回事。如果你还记得小学的数学课，就会想起不要除以 0，因为那是没有意义的。

然后再看看这种方法的其他问题，看下面的代码：

```javascript
function swapWithMulDiv(num1,num2){
  console.log(num1,num2)

  num1 = num1*num2;
  num2 = num1/num2;
  num1 = num1/num2;

  console.log(num1,num2)
}
//看看会发生什么
swapWithMulDiv(2.34,Infinity)
```

没错，又是 **NaN**。因为你无法使用 `Infinity` 去除任何值，它是未定义的。

但我还想再试试：

```javascript
function swapWithMulDiv(num1,num2){
  console.log(num1,num2)

  num1 = num1*num2;
  num2 = num1/num2;
  num1 = num1/num2;

  console.log(num1,num2)
}

//会怎样呢
swapWithMulDiv(2.34,-Infinity)
```

**-Infinity** 的结果与前面的代码相同，原因也一样。

事实证明，即使你是一位出色的“**数学家**”，也有无能为力的时候。

下面是用 `*` 和 `/` 进行值交换的较短版本，仍存在相同的问题：

```javascript
function swapWithMulDivShort(num1,num2){
  console.log(num1,num2)

  num2 = num1*(num1=num2)/num2;

  console.log(num1,num2)
}

swapWithMulDivShort(2.3,3.4)
```

上面的代码类似于用 `+` 和 `-` 进行交换时的较短的代码。把 `num2` 赋值给 `num1`，然后第 4 行的演算逻辑是这样：

```javascript
num2 = num1 * num2 / num2
    => num2 = num1
```

这样两个值就互换了。

## 5 仅使用 * 或 / 运算符

```javascript
function swapWithMul(num1,num2){
  console.log(num1,num2)

  num2 = num1 * (num1=num2, 1)

  console.log(num1,num2)
}

//Try with / and ** operator
swapWithMul(2.3,3.4)
```

上面的程序是有效的，但牺牲了可读性。在第 4 行的 `()` 中，我们将 `num1` 赋值给 `num2`，旁边的 `1` 是返回值。简而言之，第 4 行的逻辑如下所示：

```javascript
num2 = num1 * 1 
    => num2 = num1
```

这样就得到了结果。

## 6 使用按位异或（XOR）。

XOR 用来进行二进制位运算。当有两个不同的输入时，它的结果为 1，否则为 0。

|  X   |  Y   | X^Y  |
| :--: | :--: | :--: |
|  1   |  1   |  0   |
|  1   |  0   |  1   |
|  0   |  1   |  1   |
|  0   |  0   |  0   |

先了解其工作原理！

```javascript
function swapWithXOR(num1,num2){
  console.log(num1,num2)

  num1 = num1^num2;
  num2 = num1^num2; 
  num1 = num1^num2;

  console.log(num1,num2)
}
// 试试负值会怎样
swapWithXOR(10,1)
```

10 的4 位二进制数 -> 1010

1 的 4 位二进制数 -> 0001

现在：

```javascript
第四行: 
num1 = num1 ^ num2 
    => 1010 ^ 0001 
    => 1011 
    => 7 
第五行: 
num2 = num1 ^ num2 
    => 1011 ^ 0001 
    => 1010 
    => 10
第六行: 
num1 = num1 ^ num2 
    => 1011 ^ 1010 
    => 0001 
    => 1
```

两个值交换了。

再来看另一个例子：

```javascript
function swapWithXOR(num1,num2){
  console.log(num1,num2)

  num1 = num1^num2;
  num2 = num1^num2;
  num1 = num1^num2;

  console.log(num1,num2)
}

swapWithXOR(2.34,3.45)
```

嗯？？交换的值在哪儿？我们只是得到了数字的整数部分，这就是问题所在。 XOR 假定输入是整数，所以···相应地执行计算。但是浮点数不是整数，而是由 IEEE 754 标准表示的，将数字分为三部分：**符号**位、代表**指数**的一组位和代表**尾数**的一组位。位数是介于1（含）和2（不含）之间的数字。所以得到的值不正确。

另一个例子：

```javascript
function swapWithXOR(num1,num2){
  console.log(num1,num2)

  num1 = num1^num2;
  num2 = num1^num2;
  num1 = num1^num2;

  console.log(num1,num2)
}
// 试试 infinities 和整数值.
swapWithXOR(-Infinity,Infinity)
```

毫无意外，我们没有得到预期的结果。这是因为 **Infinity** 和 **– Infinity** 都是浮点数。正如我们在前面所讨论的，对于 XOR，浮点数是一个问题。

## 7 使用按位同或 （XNOR）

它用来进行二进制位运算，但是与 XOR 正好相反。当有两个不同的输入时，XNOR 的结果是 0，否则结果为 1。 JavaScript 没有执行 XNOR 的运算符，所以要用 **NOT** 运算符对 XOR 的结果求反。

|  X   |  Y   | XNOR |
| :--: | :--: | :--: |
|  1   |  1   |  1   |
|  1   |  0   |  0   |
|  0   |  1   |  0   |
|  0   |  0   |  1   |

先了解其工作原理：

```javascript
function swapWithXNOR(num1,num2){
  console.log(num1,num2)

  num1 = ~(num1^num2);
  num2 = ~(num1^num2);
  num1 = ~(num1^num2);

  console.log(num1,num2)
}

//可以试试负值
swapWithXNOR(10,1)
```

10 的 4 位二进制数 -> 1010

1 的 4 位二进制数 -> 0001

**第 4 行：**

```javascript
num1 = ~(num1 ^ num2) 
    => ~(1010 ^ 0001) 
    =>~(1011) 
    => ~11 
    => -12
```

由于这是一个负数，所以需要将其转换回二进制并计算 2 的补码来获取十进制值，例如：

```javascript
-12 => 1100 
    => 0011 + 1 
    => 0100
```

**第 5 行：**

```javascript
num2 = ~(num1 ^ num2) 
    => ~(0100 ^ 0001) 
    => ~(0101) 
    => ~5 
    => -6-6 
    => 0110 
    => 1001 + 1
    => 1010 
    => 10
```

**第 6 行：**

```javascript
num1 = ~(num1 ^ num2) 
    => ~(0100^ 1010) 
    => ~(1110) 
    => ~14 
    => -15-15 
    => 1111 
    => 0000 + 1 
    => 0001 
    => 1
```

花了一些时间，但还是交换了值。但不幸的是，它遇到了与 XOR 相同的问题，不能处理浮点数和无穷大。

试试下面的值：

```javascript
function swapWithXNOR(num1,num2){
  console.log(num1,num2)

  num1 = ~(num1^num2);
  num2 = ~(num1^num2);
  num1 = ~(num1^num2);

  console.log(num1,num2)
}

swapWithXNOR(2.3,4.5)
```

## 8 在数组中进行赋值

这是一线技巧。只需要一行代码就可以进行交换，更重要的是，无需数学运算，只需要数组的基本知识。不过它看上去可能很奇怪。

先让看看它的实际效果：

```javascript
function swapWithArray(num1,num2){
  console.log(num1,num2)

  num2 = [num1, num1 = num2][0];

  console.log(num1,num2)
}

swapWithArray(2.3,Infinity)
```

在数组的下标 0 位置中存储 `num1`，在下标 1 中，既将 `num2` 分配给 `num1`，又存储了 `num2`。另外，我们只是访问 `[0]`，将数组中的 `num1` 值存储在 `num2` 中。而且可以在这里交换我们想要的任何东西，比如：整数、浮点数（包括无穷数）以及字符串。看上去很整洁，但是在这里失去了代码的清晰度。

## 9 使用解构表达式

这是 ES6 的功能。这是所有方法中最简单的。只需要一行代码就可以完成交换：

```javascript
let num1 = 23.45;
let num2 = 45.67;

console.log(num1,num2);

[num1,num2] = [num2,num1];

console.log(num1,num2);
```

## 10、使用立即调用的函数表达式（IIFE）

这是最奇怪的一个。简单的说 IIFE 是在在定义后立即执行的函数。

可以用它来交换两个值：

```javascript
function swapWithIIFE(num1,num2){
  console.log(num1,num2)

  num1 = (function (num2){ return num2; })(num2, num2=num1)

  console.log(num1,num2)
}

swapWithIIFE(2.3,3.4)
```

在上面的例子中，在第4行立即调用一个函数。最后的括号是该函数的参数。第二个参数将 `num1` 赋值给 `num2`，仅仅返回第一个参数，不过这种交换方法效率不高。