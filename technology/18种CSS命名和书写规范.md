### 18种CSS命名和书写规范

> 选择器的命名规范

#### 1.模块化命名

例如：

- 与布局相关的样式以“g”为开头。如“g-content”和“g-header”；
- 与挂钩相关的样式以“j”为开头。如“j-open”和“j-request”；
- 与元件相关的样式以“m”为开头。如“m-dropMenu”和“m-slider”；
- 与状态相关的样式以“s”为开头。如“s-current”和“s-selected”；
- 与工具相关的样式以“u”为开头。如“u-clearfix”和“u-ellipsis”。

> “工具”是指与业务逻辑解耦的，能够重用的样式；“元件”是指自定义的可重用且可移植的基本网页元素；“挂钩”是指供JavaScript操纵的样式。

以上的说明只是举例，大家可以根据项目需求自定义开头的字符，这样做的目的是使CSS代码整洁易维护。

#### 2.选择器皆为小写形式

推荐的写法：

```css
.g-first-header
{
    line-height: 16px;
}
```

不推荐的写法：

```css
.g-FirstHeader
{
    line-height: 16px;
}
```

#### 3.每个选择器独占一列

除最后一个选择器外，其它每一列选择器均以逗号结尾。若用到兄弟元素选择器，则相关符号的左右两端均留出一个半角空格。

推荐的写法：

```css
// web前端开发学习Q-q-u-n： 731771211，分享学习的方法和需要注意的小细节，不停更新最新的教程和学习方法（详细的前端项目实战教学视频，PDF）
.g-first-header,
.g-second-header-1 > .g-second-header-2
{
    border: 2px solid #C3C3C3;
}
```

不推荐的写法：

```css
.g-first-header, .g-second-header-1>.g-second-header-2
{
    border: 2px solid #C3C3C3;
}
```

#### 4.避开HTML标记

构建选择器时应尽量采用语义明确的类别名称，避开HTML标记，因为一旦HTML的结构产生更动，则与此对应的样式也就无效了。尽量将样式与结构分离，这样会使得阶层式样式表在日后更易被维护。

推荐的写法：

```css
.g-content .g-item
{
    flex-basis: 20%;
}
```

不推荐的写法：

```css
.g-content li
{
    flex-basis: 20%;
}
```

#### 5.少用ID

ID的唯一性注定了它所对应的元素的样式就是一次性的，无法重用，一旦HTML结构发生变化，套用ID的选择器就要随之修改。另一个重要的原因是：ID的权重值是最高的，这可能会导致日后添加的样式无法复写原先的样式。

推荐的写法：

```css
.g-special-content
{
    height: 100px;
    width: 300px;
}
```

不推荐的写法：

```css
#special-content
{
    height: 100px;
    width: 300px;
}
```

> 属性的书写规范

#### 1.按顺序排列属性

每条规则下的属性在书写时，应按类别进行分组，其排列顺序如下：

1. 位置：bottom、float、display、left、position、right、top和z-index等；
2. 大小：height、margin、padding和width等；
3. 版式：color、font、letter-spacing、line-height和text-align等；
4. 背景：background等；
5. 其它：animation和transition等。

#### 2.缩写属性

有些属性是可以合在一块的，既精简代码，又便于阅读。

推荐的写法：

```css
.test-selector-1
{
    padding: 3px 5px;
}
```

不推荐的写法：

```css
.test-selector-1
{
    padding-top: 3px;
    padding-right: 5px;
    padding-bottom: 3px;
    padding-left: 5px;
}
```

#### 3.去除小数开头的0

推荐的写法：

```css
.test-selector-2
{
    font-size: .5em;
}
```

不推荐的写法：

```css
.test-selector-2
{
    font-size: 0.5em;
}
```

#### 4.缩写十六进位值

推荐的写法：

```css
.test-selector-3
{
    background-color: #0b0;
}
```

不推荐的写法：

```css
.test-selector-3
{
    background-color: #00bb00;
}
```

#### 5.合理使用引号

对于“font-family”属性来说，我们通常会以引号夹住带有空格的字体名称，而对于不具备这些特征的一般字体来说，引号存在与否并不影响页面的显示效果。为了保证视觉上的统一，最大程度相容各种浏览器，建议你在所有字体名称的两端均加上引号。

推荐的写法：

```css
.test-selector-4
{
    font-family: "Microsoft YaHei", "微软正黑体", "\5b8b\4f53";
}
```

不推荐的写法：

```css
.test-selector-4
{
    font-family: "Microsoft YaHei", 微软正黑体, \5b8b\4f53;
}
```

个别属性的值含有“url（）”字串，开发者需要往其中传入一个资源路径。请注意，在低版本的Internet Explorer中，路径中的空格有可能无法被辨识，导致资源无法被找到。为保险起见，不论路径中是否含有空格，你传入的路径两端最好都加上引号。

推荐的写法：

```css
//web前端开发学习Q-q-u-n： 731771211，分享学习的方法和需要注意的小细节，不停更新最新的教程和学习方法（详细的前端项目实战教学视频，PDF）
.test-selector-5
{
    background-image: url(“../Images/BacPic.png”);
}
```

不推荐的写法：

```css
.test-selector-5
{
    background-image: url(../Images/BackPic.png);
}
```

#### 6.避开！important

“！important”会给日后的维护带来麻烦，使开发者难以查找样式问题。如果在书写时发现新样式无法复写旧样式。通常有两个原因：要么新样式写在了旧样式的前面，要么新样式对应的选择器的权重比旧样式的更低。针对后一种情况，只要增加新样式选择器的权重值就可以完全避开这个问题，无需用到“！important”。

推荐的写法：

```css
.test-selector-6 .test-selector-7
{
    font-size: 16px;
}

.test-selector-6 .test-selector-7 .test-selector-8
{
    font-size: 14px;
}
```

不推荐的写法：

```css
.test-selector-6 .test-selector-7
{
    font-size: 16px;
}

.test-selector-8
{
    font-size: 14px !important;
}
```

#### 7.规范注释

在单列注释中，星号与内容之间留一个半角空格。

推荐的写法：

```css
/* 这是第一段注释文字。 */

// 这是第二段注释文字。复制代码
```

不推荐的写法：

```css
/*这里是一段注释文字。*/

//这是第二段注释文字。复制代码
```

在多列注释中，多个星号要排成一条线。星号与内容之间同样留一个半角空格。

推荐的写法：

```css
/**
 * 这里是一段注释文字。
 * 这是第二段注释文字。
 */
```

不推荐的写法：

```css
/**
*这里是一段注释文字。
*这是第二段注释文字。
*/
```

在文档注释中，除了要按照多列注释的写法以外，还要用标识符来说明文档中的某一部分，标识符后的冒号右侧与说明文字之间留一个半角空格。

推荐的写法：

```css
/**

* @name: 文件名；

* @description: 描述文字；

* @author: 张三、李四；

* @update: 2018年12月19日。

*/
```

不推荐的写法：

```css
/**

* @name:文件名；

* @description:描述文字；

* @author:张三、李四；

* @update:2018年12月19日。

*/
```

#### 8.将标准属性置于底部

有些属性在部分浏览器中尚未完全标准化，每家浏览器开发商对这些属性的实现效果或许并不统一，因此目前需要在开头加入浏览器厂商的专有字符串。因此同一个属性需要写多次，但有一条需要注意：将不带前置标记的属性置于最下方。

推荐的写法：

```css
.test-selector-9
{
    opacity: 0;
    -webkit-transition: opacity 3s;
    -moz-transition: opacity 3s;
    -ms-transition: opacity 3s;
    -o-transition: opacity 3s;
    transition: opacity 3s;
}
```

不推荐的写法：

```css
.test-selector-9
{
    opacity: 0;
    -webkit-transition: opacity 3s;
    transition: opacity 3s;
    -moz-transition: opacity 3s;
    -ms-transition: opacity 3s;
    -o-transition: opacity 3s;
}
```

#### 9.注意标点符号

每个属性独占一列。紧接样式属性的冒号，其后面要留一个半角空格。值以分号结尾。

推荐的写法：

```css
.test-selector-10
{
    opacity: .5;
}
```

不推荐的写法：

```css
.test-selector-10
{
    opacity:.5
}
```

#### 10.样式块间留一空行

样式选择器及其样式块与周遭内容要保留一空行以避免内容过于拥挤，妨碍寻找。

推荐的写法：

```css
.test-selector-11
{
    opacity: 0.5;
}

.test-selector-12
{
    font-size: 16px;
}

.test-selector-13
{
    overflow: hidden;
}
```

不推荐的写法：

```css
.test-selector-11
{
    opacity: 0.5;
}
.test-selector-12
{
    font-size: 16px;
}
.test-selector-13
{
    overflow: hidden;
}
```

#### 11.将过长的内容折为若干列

同一属性的值不止一个或值过长时，以逗号分割这些值，每个逗号后添加一个空格，过长的值可以另起一列。

推荐的写法：

```css
//web前端开发学习Q-q-u-n： 731771211，分享学习的方法和需要注意的小细节，不停更新最新的教程和学习方法（详细的前端项目实战教学视频，PDF）
.test-selector-14
{
    linear-gradient(135deg, deeppink 25%, transparent 25%) - 50px 0,
    linear-gradient(225deg, deeppink 25%, transparent 25%) - 50px 0,
    linear-gradient(315deg, deeppink 25%, transparent 25%),
    linear-gradient(45deg, deeppink 25%, transparent 25%);
}
```

不推荐的写法：

```css
.test-selector-14
{
    linear-gradient(135deg, deeppink 25%, transparent 25%) - 50px 0, linear-gradient(225deg, deeppink 25%, transparent 25%) - 50px 0, linear-gradient(315deg, deeppink 25%, transparent 25%), linear-gradient(45deg, deeppink 25%, transparent 25%);
}
```

#### 12.避开CSS Hack

所谓“CSS Hack”，就是在样式表中加入少许特殊符号，让能够辨识不同符号的浏览器在同一个元素上计算出来的样式各不相同。出现CSS Hack的原因就在于老式的浏览器（诸如饱受诟病的Internet Explorer 6）对同一套样式表的计算结果与其它浏览器的并不相同，这就很有可能会造成版式上的错乱。因此在过去，我们通常要针对个别怪异的浏览器撰写有针对性的CSS。如width: 300px; _width: 200px;对其它浏览器来说，该选择器的宽度值应为300个像素，但IE 6能够辨识出底线，因此它计算出的宽度就是200个像素。

#### 13.减少使用影响性能的属性

样式表中不要含有过多的滤镜表达式和repeat关键字等，这些属性会降低网页的渲染性能。若要重复背景图片，那么原图的宽高各不小于8px。