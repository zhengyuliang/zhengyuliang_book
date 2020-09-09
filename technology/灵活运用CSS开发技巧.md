### [灵活运用CSS开发技巧](https://segmentfault.com/a/1190000020899202)

### 前言

何为技巧，意指表现在文学、工艺、体育等方面的巧妙技能。代码作为一门现代高级工艺，推动着人类科学技术的发展，同时犹如文字一样承托着人类文化的进步。

每写好一篇文章，都会使用大量的写作技巧。`烘托、渲染、悬念、铺垫、照应、伏笔、联想、想象、抑扬结合、点面结合、动静结合、叙议结合、情景交融、首尾呼应、衬托对比、白描细描、比喻象征、借古讽今、卒章显志、承上启下、开门见山、动静相衬、虚实相生、实写虚写、托物寓意、咏物抒情`等，这些应该都是我们从小到大写文章而接触到的写作技巧。

作为程序猿的我们，写代码同样也需要大量的写作技巧。一份良好的代码能让人耳目一新，让人容易理解，让人舒服自然，同时也让自己成就感满满(哈哈，这个才是重点)。因此，我整理下三年来自己使用到的一些**CSS开发技巧**，希望能让你写出耳目一新、容易理解、舒服自然的代码。

### 目录

既然写文章有这么多的写作技巧，那么我也需要对**CSS开发技巧**整理一下，起个易记的名字。

- **Layout Skill**：布局技巧
- **Behavior Skill**：行为技巧
- **Color Skill**：色彩技巧
- **Figure Skill**：图形技巧
- **Component Skill**：组件技巧

备注

- 代码只作演示用途，不会详细说明语法
- 部分技巧示例代码过长，使用`CodePen`进行保存，点击**在线演示**即可查看
- 兼容项点击链接即可查看当前属性的浏览器兼容数据，自行根据项目兼容需求考虑是否使用
- 以下代码全部基于CSS进行书写，没有任何JS代码，没有特殊说明的情况下所有属性和方法都是CSS类型
- 一部分技巧是自己探讨出来的，另一部分技巧是参考各位前端大神们的，都是一个互相学习的过程，大家一起进步

### Layout Skill

##### 使用vw定制rem自适应布局

- 要点：移动端使用`rem布局`需要通过JS设置不同屏幕宽高比的`font-size`，结合`vw`单位和`calc()`可脱离JS的控制
- 场景：**rem页面布局**(不兼容低版本移动端系统)
- 兼容：[vw](https://caniuse.com/#search=vw)、[calc()](https://caniuse.com/#search=calc())

```css
/* 基于UI width=750px DPR=2的页面 */
html {
    font-size: calc(100vw / 7.5);
}
```

##### 使用:nth-child()选择指定元素

- 要点：通过`:nth-child()`筛选指定的元素设置样式
- 场景：**表格着色**、**边界元素排版**(首元素、尾元素、左右两边元素)
- 兼容：[:nth-child()](https://caniuse.com/#search=%3Anth-child())
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/voRzNP)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899205)

##### 使用writing-mode排版竖文

- 要点：通过`writing-mode`调整文本排版方向
- 场景：**竖行文字**、**文言文**、**诗词**
- 兼容：[writing-mode](https://caniuse.com/#search=writing-mode)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/XvExJO)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899206)

##### 使用text-align-last对齐两端文本

- 要点：通过`text-align-last:justify`设置文本两端对齐
- 场景：**未知字数中文对齐**
- 兼容：[text-align-last](https://caniuse.com/#search=text-align-last)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/ZgxZJa)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899207)

##### 使用:not()去除无用属性

- 要点：通过`:not()`排除指定元素不使用设置样式
- 场景：**符号分割文字**、**边界元素排版**(首元素、尾元素、左右两边元素)
- 兼容：[:not()](https://caniuse.com/#search=%3Anot())
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/gVeyqr)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899208)

##### 使用object-fit规定图像尺寸

- 要点：通过`object-fit`使图像脱离`background-size`的约束，使用`<img>`来标记图像背景尺寸
- 场景：**图片尺寸自适应**
- 兼容：[object-fit](https://caniuse.com/#search=object-fit)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/LwBKLV)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899209)

##### 使用overflow-x排版横向列表

- 要点：通过`flexbox`或`inline-block`的形式横向排列元素，对父元素设置`overflow-x:auto`横向滚动查看
- 场景：**横向滚动列表**、**元素过多但位置有限的导航栏**
- 兼容：[overflow-x](https://caniuse.com/#search=overflow-x)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/jONqyVd)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899210)

##### 使用text-overflow控制文本溢出

- 要点：通过`text-overflow:ellipsis`对溢出的文本在末端添加`...`
- 场景：**单行文字溢出**、**多行文字溢出**
- 兼容：[text-overflow](https://caniuse.com/#search=text-overflow)、[line-clamp](https://caniuse.com/#search=line-clamp)、[box-orient](https://www.w3school.com.cn/cssref/pr_box-orient.asp)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/mdbPmyy)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899211)

##### 使用transform描绘1px边框

- 要点：分辨率比较低的屏幕下显示1px的边框会显得模糊，通过`::before`或`::after`和`transform`模拟细腻的1px边框
- 场景：**容器1px边框**
- 兼容：[transform](https://caniuse.com/#search=transform)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/YzKqMVO)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899212)

##### 使用transform翻转内容

- 要点：通过`transform:scale3d()`对内容进行翻转(水平翻转、垂直翻转、倒序翻转)
- 场景：**内容翻转**
- 兼容：[transform](https://caniuse.com/#search=transform)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/NWKNZwO)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899213)

##### 使用letter-spacing排版倒序文本

- 要点：通过`letter-spacing`设置负值字体间距将文本倒序
- 场景：**文言文**、**诗词**
- 兼容：[letter-spacing](https://caniuse.com/#search=letter-spacing)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/zYOBgqB)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899214)

##### 使用margin-left排版左重右轻列表

- 要点：使用`flexbox横向布局`时，最后一个元素通过`margin-left:auto`实现向右对齐
- 场景：**右侧带图标的导航栏**
- 兼容：[margin](https://caniuse.com/#search=margin)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/PoYpROw)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899215)

### Behavior Skill

##### 使用overflow-scrolling支持弹性滚动

- 要点：iOS页面`非body元素`的滚动操作会非常卡(Android不会出现此情况)，通过`overflow-scrolling:touch`调用Safari原生滚动来支持弹性滚动，增加页面滚动的流畅度
- 场景：**iOS页面滚动**
- 兼容：iOS自带`-webkit-overflow-scrolling`

```css
body {
    -webkit-overflow-scrolling: touch;
}
.elem {
    overflow: auto;
}
```

##### 使用transform启动GPU硬件加速

- 要点：有时执行动画可能会导致页面卡顿，可在特定元素中使用硬件加速来避免这个问题
- 场景：**动画元素**(绝对定位、同级中超过6个以上使用动画)
- 兼容：[transform](https://caniuse.com/#search=transform)

```css
.elem {
    transform: translate3d(0, 0, 0); /* translateZ(0)亦可 */
}
```

##### 使用attr()抓取data-*

- 要点：在标签上自定义属性`data-*`，通过`attr()`获取其内容赋值到`content`上
- 场景：**提示框**
- 兼容：[data-*](https://caniuse.com/#search=data-)、[attr()](https://caniuse.com/#search=attr())
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/voRdKX)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899216)

##### 使用:valid和:invalid校验表单

- 要点：`<input>`使用伪类`:valid`和`:invalid`配合`pattern`校验表单输入的内容
- 场景：**表单校验**
- 兼容：[pattern](https://caniuse.com/#search=pattern)、[:valid](https://caniuse.com/#search=%3Avalid)、[:invalid](https://caniuse.com/#search=%3Ainvalid)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/QemxKr)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899218)

##### 使用pointer-events禁用事件触发

- 要点：通过`pointer-events:none`禁用事件触发(默认事件、冒泡事件、鼠标事件、键盘事件等)，相当于`<button>`的`disabled`
- 场景：**限时点击按钮**(发送验证码倒计时)、**事件冒泡禁用**(多个元素重叠且自带事件、a标签跳转)
- 兼容：[pointer-events](https://caniuse.com/#search=pointer-events)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/dxmrLj)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899219)

##### 使用+或~美化选项框

- 要点：`<label>`使用`+`或`~`配合`for`绑定`radio`或`checkbox`的选择行为
- 场景：**选项框美化**、**选中项增加选中样式**
- 兼容：[+](https://caniuse.com/#search=+)、[~](https://caniuse.com/#search=~)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/rXdbgZ)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899220)

##### 使用:focus-within分发冒泡响应

- 要点：表单控件触发`focus`和`blur`事件后往父元素进行冒泡，在父元素上通过`:focus-within`捕获该冒泡事件来设置样式
- 场景：**登录注册弹框**、**表单校验**、[**离屏导航**](https://codepen.io/dannievinther/pen/NvZjvz)点击预览、[**导航切换**](https://codepen.io/Chokcoco/pen/RJEpaP)点击预览
- 兼容：[:focus-within](https://www.caniuse.com/#search=%3Afocus-within)、[:placeholder-shown](https://www.caniuse.com/#search=%3Aplaceholder-shown)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/BaBjaBP)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899221)

##### 使用:hover描绘鼠标跟随

- 要点：将整个页面等比划分成小的单元格，每个单元格监听`:hover`，通过`:hover`触发单元格的样式变化来描绘鼠标运动轨迹
- 场景：**鼠标跟随轨迹**、[**水波纹**](https://codepen.io/YusukeNakaya/pen/vvEqVx)点击预览、[**怪圈**](https://codepen.io/Chokcoco/pen/zyyYqN)点击预览
- 兼容：[:hover](https://www.caniuse.com/#search=%3Ahover)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/wvwMLJY)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899222)

##### 使用max-height切换自动高度

- 要点：通过`max-height`定义收起的最小高度和展开的最大高度，设置两者间的过渡切换
- 场景：**隐藏式子导航栏**、**悬浮式折叠面板**
- 兼容：[max-height](https://caniuse.com/#search=max-height)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/NQYJpm)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899223)

##### 使用transform模拟视差滚动

- 要点：通过`background-attachment:fixed`或`transform`让多层背景以不同的速度移动，形成立体的运动效果
- 场景：[**页面滚动**](https://codepen.io/Chokcoco/pen/JBaQoY)点击预览、[**视差滚动文字阴影**](https://codepen.io/Chokcoco/pen/XBgBBp)点击预览、[**视差滚动文字虚影**](https://codepen.io/Chokcoco/pen/PBXwdX)点击预览
- 兼容：[background-attachment](https://www.caniuse.com/#search=background-attachment)、[transform](https://www.caniuse.com/#search=transform)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/MWgaBoK)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899224)

##### 使用animation-delay保留动画起始帧

- 要点：通过`transform-delay`或`animation-delay`设置负值时延保留动画起始帧，让动画进入页面不用等待即可运行
- 场景：**开场动画**
- 兼容：[transform](https://www.caniuse.com/#search=transform)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/WNexVoB)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899225)

##### 使用resize拉伸分栏

- 要点：通过`resize`设置横向自由拉伸来调整目标元素的宽度
- 场景：**富文本编辑器**、**分栏阅读**
- 兼容：[resize](https://caniuse.com/#search=resize)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/JjPEdWO)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899226)

### Color Skill

##### 使用color改变边框颜色

- 要点：`border`没有定义`border-color`时，设置`color`后，`border-color`会被定义成`color`
- 场景：**边框颜色与文字颜色相同**
- 兼容：[color](https://caniuse.com/#search=color)

```css
.elem {
    border: 1px solid;
    color: #f66;
}
```

![在线演示](https://segmentfault.com/img/remote/1460000020899227)

##### 使用filter开启悼念模式

- 要点：通过`filter:grayscale()`设置灰度模式来悼念某位去世的仁兄或悼念因灾难而去世的人们
- 场景：**网站悼念**
- 兼容：[filter](https://caniuse.com/#search=filter)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/vYBKqwe)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899228)

##### 使用::selection改变文本选择颜色

- 要点：通过`::selection`根据主题颜色自定义文本选择颜色
- 场景：**主题化**
- 兼容：[::selection](https://caniuse.com/#search=%3A%3Aselection)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/jONrjXX)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899229)

##### 使用linear-gradient控制背景渐变

- 要点：通过`linear-gradient`设置背景渐变色并放大背景尺寸，添加背景移动效果
- 场景：**主题化**、**彩虹背景墙**
- 兼容：[gradient](https://caniuse.com/#search=gradient)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/oNvbRwN)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899230)

##### 使用linear-gradient控制文本渐变

- 要点：通过`linear-gradient`设置背景渐变色，配合`background-clip:text`对背景进行文本裁剪，添加滤镜动画
- 场景：**主题化**、**特色标题**
- 兼容：[gradient](https://caniuse.com/#search=gradient)、[background-clip](https://www.caniuse.com/#search=background-clip)、[filter](https://caniuse.com/#search=filter)、[animation](https://www.caniuse.com/#search=animation)、[text-fill-color](https://www.caniuse.com/#search=text-fill-color)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/pozgQVo)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899231)

##### 使用caret-color改变光标颜色

- 要点：通过`caret-color`根据主题颜色自定义光标颜色
- 场景：**主题化**
- 兼容：[caret-color](https://caniuse.com/#search=caret-color)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/QemxKr)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899232)

##### 使用::scrollbar改变滚动条样式

- 要点：通过`scrollbar`的`scrollbar-track`和`scrollbar-thumb`等属性来自定义滚动条样式
- 场景：**主题化**、**页面滚动**
- 兼容：[::scrollbar](https://www.caniuse.com/#search=scrollbar)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/ExYPMog)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899233)

##### 使用filter模拟Instagram滤镜

- 要点：通过`filter`的滤镜组合起来模拟`Instagram滤镜`
- 场景：**图片滤镜**
- 兼容：[filter](https://caniuse.com/#search=filter)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/NWKbVNQ)点击预览、[css-gram](https://github.com/una/CSSgram/blob/master/README-CN.md)

![在线演示](https://segmentfault.com/img/remote/1460000020899234)

### Figure Skill

##### 使用div描绘各种图形

- 要点：`<div>`配合其伪元素(`::before`、`::after`)通过`clip`、`transform`等方式绘制各种图形
- 场景：各种图形容器
- 兼容：[clip](https://caniuse.com/#search=clip)、[transform](https://caniuse.com/#search=transform)
- 代码：[在线演示](https://css-tricks.com/the-shapes-of-css/)

##### 使用mask雕刻镂空背景

- 要点：通过`mask`为图像背景生成蒙层提供遮罩效果
- 场景：**高斯模糊蒙层**、[**票劵**(电影票、购物卡)](https://codepen.io/HelKyle/pen/XxZPmY/)点击预览、[**遮罩动画**](https://codepen.io/banik/pen/aRpvdW)点击预览
- 兼容：[mask](https://www.caniuse.com/#search=mask)、[perspective](https://caniuse.com/#search=perspective)、[transform-style](https://caniuse.com/#search=transform-style)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/xxKZdZN)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899235)

##### 使用linear-gradient描绘波浪线

- 要点：通过`linear-gradient`绘制波浪线
- 场景：**文字强化显示**、**文字下划线**、**内容分割线**
- 兼容：[gradient](https://caniuse.com/#search=gradient)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/EqEzwq)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899236)

##### 使用linear-gradient描绘彩带

- 要点：通过`linear-gradient`绘制间断颜色的彩带
- 场景：**主题化**
- 兼容：[gradient](https://caniuse.com/#search=gradient)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/bGbeXZG)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899237)

##### 使用conic-gradient描绘饼图

- 要点：通过`conic-gradient`绘制多种色彩的饼图
- 场景：**项占比饼图**
- 兼容：[gradient](https://caniuse.com/#search=gradient)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/XWrjrgE)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899238)

##### 使用linear-gradient描绘方格背景

- 要点：使用`linear-gradient`绘制间断颜色的彩带进行交互生成方格
- 场景：**格子背景**、**占位图**
- 兼容：[gradient](https://caniuse.com/#search=gradient)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/RwboXoV)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899239)

##### 使用box-shadow描绘单侧投影

- 要点：通过`box-shadow`生成投影，且模糊半径和负的扩张半径一致，使投影偏向一侧
- 场景：**容器投影**、[**背景补间动画1**](https://codepen.io/Chokcoco/pen/WaBYZL)点击预览、[**背景补间动画2**](https://codepen.io/davidkpiano/pen/LVzxPV)点击预览、[**立体投影**](https://codepen.io/Chokcoco/pen/LgdRKE)点击预览、[**文字立体投影**](https://codepen.io/Chokcoco/pen/JmgNNa)点击预览、[**文字渐变立体投影**](https://codepen.io/Chokcoco/pen/XxQJEB)点击预览、[**长投影**](https://codepen.io/Chokcoco/pen/qJvVGy)点击预览、[**霓虹灯**](https://codepen.io/Chokcoco/pen/WaLdwX)点击预览、[**灯光阴影**](https://codepen.io/Chokcoco/pen/ReOgvq)点击预览
- 兼容：[box-shadow](https://caniuse.com/#search=box-shadow)、[filter](https://caniuse.com/#search=filter)、[text-shadow](https://caniuse.com/#search=text-shadow)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/BaBLqYo)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899240)

##### 使用filter描绘头像彩色阴影

- 要点：通过`filter:blur() brightness() opacity()`模拟阴影效果
- 场景：**头像阴影**
- 兼容：[filter](https://caniuse.com/#search=filter)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/GRKjYap)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899241)

##### 使用box-shadow裁剪图像

- 要点：通过`box-shadow`模拟蒙层实现中间镂空
- 场景：**图片裁剪**、**新手引导**、**背景镂空**、**投射定位**
- 兼容：[box-shadow](https://caniuse.com/#search=box-shadow)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/zYONxRG)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899242)

##### 使用outline描绘内边框

- 要点：通过`outline`设置轮廓进行描边，可设置`outline-offset`设置内描边
- 场景：**内描边**、**外描边**
- 兼容：[outline](https://caniuse.com/#search=outline)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/pozeVyL)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899243)

### Component Skill

##### 迭代计数器

- 要点：累加选项单位的计数器
- 场景：**章节目录**、**选项计数器**、[**加法计数器**](https://codepen.io/CSSKing/pen/vEeMey)点击预览
- 兼容：[counters](https://caniuse.com/#search=counters)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/rXqRPo)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899244)

##### 下划线跟随导航栏

- 要点：下划线跟随鼠标移动的导航栏
- 场景：**动态导航栏**
- 兼容：[+](https://caniuse.com/#search=+)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/eYOJbNv)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899245)

##### 气泡背景墙

- 要点：不间断冒出气泡的背景墙
- 场景：**动态背景**
- 兼容：[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/GRKoPdK)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899246)

##### 滚动指示器

- 要点：提示滚动进度的指示器
- 场景：[**阅读进度**](https://codepen.io/MadeByMike/pen/ZOrEmr)点击预览
- 兼容：[calc()](https://caniuse.com/#search=calc())、[gradient](https://caniuse.com/#search=gradient)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/ExYPMog)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899247)

##### 故障文本

- 要点：显示器故障形式的文本
- 场景：**错误提示**
- 兼容：[data-*](https://caniuse.com/#search=data-)、[attr()](https://caniuse.com/#search=attr())、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/xxKZNYv)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899248)

##### 换色器

- 要点：通过拾色器改变图像色相的换色器
- 场景：**图片色彩变换**
- 兼容：[mix-blend-mode](https://www.caniuse.com/#search=mix-blend-mode)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/vYBLqBm)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899249)

##### 状态悬浮球

- 要点：展示当前状态的悬浮球
- 场景：**状态动态显示**、**波浪动画**
- 兼容：[gradient](https://caniuse.com/#search=gradient)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/WNewOxa)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899250)

##### 粘粘球

- 要点：相交粘粘效果的双球回弹运动
- 场景：[**粘粘动画**](https://codepen.io/Chokcoco/pen/QqWBqV)点击预览
- 兼容：[filter](https://caniuse.com/#search=filter)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/zYOqdBz)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899251)

##### 商城票券

- 要点：边缘带孔和中间折痕的票劵
- 场景：**电影票**、**代金券**、**消费卡**
- 兼容：[gradient](https://caniuse.com/#search=gradient)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/rNBeYza)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899252)

##### 倒影加载条

- 要点：带有渐变倒影的加载条
- 场景：**加载提示**
- 兼容：[box-reflect](https://caniuse.com/#search=box-reflect)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/GRKZzpg)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899253)

##### 三维立方体

- 要点：三维建模的立方体
- 场景：**三维建模**
- 兼容：[transform](https://caniuse.com/#search=transform)、[perspective](https://caniuse.com/#search=perspective)、[transform-style](https://caniuse.com/#search=transform-style)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/PoYNgXY)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899254)

##### 动态边框

- 要点：鼠标悬浮时动态渐变显示的边框
- 场景：**悬浮按钮**、**边框动画**
- 兼容：[gradient](https://caniuse.com/#search=gradient)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/qBWZPvE)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899255)

##### 标签页

- 要点：可切换内容的标签页
- 场景：**内容切换**
- 兼容：[scroll-behavior](https://caniuse.com/#search=scroll-behavior)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/JjPRjMd)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899256)

##### 标签导航栏

- 要点：可切换内容的导航栏
- 场景：**页面切换**
- 兼容：[~](https://caniuse.com/#search=~)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/oNvzoZg)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899257)

##### 折叠面板

- 要点：可折叠内容的面板
- 场景：**隐藏式子导航栏**
- 兼容：[~](https://caniuse.com/#search=~)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/NWKRMjo)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899258)

##### 星级评分

- 要点：点击星星进行评分的按钮
- 场景：**评分**
- 兼容：[~](https://caniuse.com/#search=~)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/MWgjGMj)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899259)

##### 加载指示器

- 要点：变换`...`长度的加载提示
- 场景：**加载提示**
- 兼容：[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/wvwoRbN)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899260)

##### 自适应相册

- 要点：自适应照片数量的相册
- 场景：**九宫格相册**、**微信相册**、**图集**
- 兼容：[:only-child](https://caniuse.com/#search=%3Aonly-child)、[:first-child](https://caniuse.com/#search=%3Afirst-child)、[:nth-child()](https://caniuse.com/#search=%3Anth-child())、[:nth-last-child()](https://caniuse.com/#search=%3Anth-last-child())、[~](https://caniuse.com/#search=~)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/pozNGyj)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899261)

##### 圆角进度条

- 要点：单一颜色的圆角进度条
- 场景：**进度条**
- 兼容：[gradient](https://caniuse.com/#search=gradient)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/jONBxaK)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899262)

##### 螺纹进度条

- 要点：渐变螺纹的进度条
- 场景：**进度条**、**加载动画**
- 兼容：[gradient](https://caniuse.com/#search=gradient)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/GRKrJJX)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899263)

##### 立体按钮

- 要点：点击呈现按下状态的按钮
- 场景：**按钮点击**
- 兼容：[box-shadow](https://caniuse.com/#search=box-shadow)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/PoYpaLL)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899264)

##### 混沌加载圈

- 要点：带混沌虚影的加载圈
- 场景：**加载提示**
- 兼容：[filter](https://caniuse.com/#search=filter)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/aboWbqG)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899265)

##### 蛇形边框

- 要点：蛇形运动的边框
- 场景：**蛇形动画**
- 兼容：[clip](https://caniuse.com/#search=clip)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/GRKmgZZ)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899266)

##### 自动打字

- 要点：逐个字符自动打印出来的文字
- 场景：**代码演示**、**文字输入动画**
- 兼容：[ch](https://caniuse.com/#search=ch)、[animation](https://www.caniuse.com/#search=animation)
- 代码：[在线演示](https://codepen.io/JowayYoung/pen/ZEzKQEx)点击预览

![在线演示](https://segmentfault.com/img/remote/1460000020899267)

### 总结

写到最后总结得差不多了，如果后续我想起还有哪些遗漏的**CSS开发技巧**，会继续在这篇文章上补全。

最后送大家一个键盘！

```
(_=>[..."`1234567890-=~~QWERTYUIOP[]\\~ASDFGHJKL;'~~ZXCVBNM,./~"].map(x=>(o+=`/${b='_'.repeat(w=x<y?2:' 667699'[x=["Bs","Tab","Caps","Enter"][p++]||'Shift',p])}\\|`,m+=y+(x+'    ').slice(0,w)+y+y,n+=y+b+y+y,l+=' __'+b)[73]&&(k.push(l,m,n,o),l='',m=n=o=y),m=n=o=y='|',p=l=k=[])&&k.join`
`)()
```