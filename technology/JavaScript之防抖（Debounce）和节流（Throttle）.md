#### JavaScript之防抖（Debounce）和节流（Throttle）

### 一、背景

防抖和节流是两种不同的控制一个函数执行次数的方法，其目的都是为了节约计算机资源。
 当我们操作DOM的时候，加上节流或者防抖就非常有必要，因为众所周知，操作DOM的开销是非常大的，所以要尽可能减少DOM操作次数。

```html
// html
<h1>Number of scroll events </h1>
<a href="#" class="reset">Reset</a>
<div id="counter">0</div>
```

```css
//css
body {
   background: #444444;
   color: white;
    font: 15px/1.51 system, -apple-system, ".SFNSText-Regular", "San Francisco", "Roboto", "Segoe UI", "Helvetica Neue", "Lucida Grande", sans-serif;
   margin:0 auto;
   max-width:600px;
   padding:20px;
   min-height:1000vh; /* 100 times viewport height */
}
#counter {
  position:fixed;
  top:100px;
  left:40%;
  font-size:50px;
}
.reset {
  color:white;
  text-decoration:none;
  border:1px solid white;
  padding:10px 20px;
  background:rgba(0,0,0,0.1);
}
```

```javascript
//js
var i = 0;
var $counter = $('#counter');
$(document).ready(function(){
  $(document).on('scroll', function(){
    $counter.html(i);
    i++; 
  });
});

$('.reset').on('click', function(){
  $counter.html('');
  i = 0;
})
```

 当鼠标滚动或者拖拽的时候可以轻易地每秒触发30个事件，而且在移动端慢速滚动可以达到每秒100次，要是这么短的时间内每次都触发一个或多个函数，那浏览器应该会卡死崩溃的，所以这就引出了**防抖**和**节流**。 



### 二、防抖（Debounce）

例子：

```html
<a class="trigger-area">Trigger area</a>
<a class="reset">Reset</a>
<div class="visualizations">
<h2>Raw events over time</h2>
<div id="raw-events" class="events"></div>
<h2>Debounced events
  <span class="details"> 400ms, trailing</span></h2>
<div id="debounced-events" class="events"></div>
</div>

```

```css
body {
   background: #444444;
   color: white;
   font: 15px/1.51 system, -apple-system, ".SFNSText-Regular", "San Francisco", "Roboto", "Segoe UI", "Helvetica Neue", "Lucida Grande", sans-serif;
   margin:0 auto;
   max-width:700px;
   padding:20px;
}

.events{
  padding:0px 20px 10px 20px;
  height: 23px;
}
.events span {
  height:17px;
  width:6px;
  display:inline-block;
  border-right:1px solid #111;

}
.events span:last-of-type {
  border:2px solid black;
  border-bottom: 4px solid #AAA;
  border-top: 0px;
  margin-bottom:-17px;
  margin-left:-2px;
}
h2 {
  margin:10px 0 5px 0;
  clear:both;
  font-weight: normal;
  font-size:14px;
  padding:6px 20px;
}

.trigger-area {
  margin: 0;
  display:inline-block;
  width: 200px;
  height:50px;
  border: 1px solid #5ed1ff;
  padding: 28px 0 0 0;
  text-align: center;
  background-color: transparent;
  cursor:pointer;
  font-size:17px;
  -webkit-user-select: none;  /* Chrome  / Safari */
  -moz-user-select: none;     /* Firefox all */
  -ms-user-select: none;      /* IE 10+ */
  user-select: none;          /* Likely future */    
}
.trigger-area.active {
  background:#2F5065;
}
.clickme:hover,
.clickme:active{
  background-color: #333;
}
.clickme:active{
  padding: 4px 5px;
}
.reset {
  display:inline-block;
  width: 120px;
  padding: 10px 0 0 0;
  text-align: center;
  font-size:14px;
  cursor:pointer;
  color:#eee;
}
.visualizations {
  margin-top:10px;
  background:rgba(0,0,0,0.2);
}
.details {
  font-size:13px;
  color:#999;
}

/* stating the obvious: color0 represents our empty color */
.color0 { transparent}

.color1 { background-color: #FFE589}
.color2 { background-color: #B9C6FF}
.color3 { background-color: #99FF7E}
.color4 { background-color: #FFB38A}
.color5 { background-color: #A5FCFF}
.color6 { background-color: #FF8E9B}
.color7 { background-color: #E3FF7E}
.color8 { background-color: #FFA3D8}
.color9 { background-color: #5ca6ff}
.color10 { background-color: #9BFFBB}
```

```javascript
$(document).ready(function(){

  var $rawDiv = $('#raw-events'),
      $debounceDiv = $('#debounced-events'),
      $triggerArea = $('.trigger-area'),
      initialized = false,
      frequency = 100,
      barLength = 0,
      globalColor = 2,
      colorNeedChange = false,
      interval_id,
      rawColor = 0,
      debounceColor = 0,
      maxBarLength = 87;
  
  var drawDebouncedEvent = _.debounce(function(div){
   debounceColor = globalColor;
  }, frequency*4, {leading:false, trailing:true});
  

  var changeDebouncedColor = _.debounce(function(div){
    // Change colors, to visualize easier the "group of events" that is reperesenting this debounced event
    
    globalColor++;
    if (globalColor > 9){
      globalColor = 2;
    } 
  }, frequency*4, {leading:false, trailing:true});
  

  function draw_tick_marks(){

      // every x seconds, draw a tick mark in the bar
      interval_id = setInterval(function(){
      barLength++;   
      $rawDiv.append('<span class="color' + rawColor + '" >');
      $debounceDiv.append('<span class="color' + debounceColor + '" >');
      rawColor = 0; // make it transparent again
      debounceColor = 0; // make it transparent again
        
      if (barLength > maxBarLength){
        clearInterval(interval_id);
      }
      
    }, frequency);
  };
  
  
  // Track Mouse movement or clicks for mobile
  $triggerArea.on('click mousemove', function (){  
    if (!initialized) {
      initialized = true;
      draw_tick_marks();
      $(this).addClass('active');
    } 
    rawColor = globalColor;
    drawDebouncedEvent();
    changeDebouncedColor();
  });

  $('.reset').on('click', function(){
    initialized = false;
    $triggerArea.removeClass('active');
    $rawDiv.empty();
    $debounceDiv.empty();
    barLength = 0;
    clearInterval(interval_id);
  });

});
```



#####  具体实现 

```javascript
function debounce(func,delay) {
    let timer;
    return function(...args) {
        if (timer) {
            clearTimeout(timer);
        }
        timer = setTimeout(() => {
            func.apply(this,arguments);
        },delay)
    }
}
```

 [Lodash](https://links.jianshu.com/go?to=https%3A%2F%2Flodash.com%2Fdocs%23debounce)中实现 `_.debounce` 和 `_.throttle` 的功能很全面，可以直接使用，其中的 `throttle`函数是使用 `_.debounce` 新增 maxWait` 选项来实现的 

#####  Resize实例 

```javascript
// Based on http://www.paulirish.com/2009/throttled-smartresize-jquery-event-handler/
$(document).ready(function(){
  
  var $win = $(window);
  var $left_panel = $('.left-panel');
  var $right_panel = $('.right-panel');
  
  function display_info($div) {
    $div.append($win.width() + ' x ' + $win.height() +  '<br>');
  }
                
  $(window).on('resize', function(){
    display_info($left_panel);
  });
  
  $(window).on('resize', _.debounce(function() {
    display_info($right_panel);
  }, 400));
});
```



#####  输入验证实例 

```javascript
$(document).ready(function(){

  var $statusKey = $('.status-key');
  var $statusAjax = $('.status-ajax');
  var intervalId;
  
  // Fake ajax request. Just for demo
  function make_ajax_request(e){
      var that = this;
      $statusAjax.html('That\'s enough waiting. Making now the ajax request');
     
      intervalId = setTimeout(function(){
         $statusKey.html('Type here. I will detect when you stop typing');
         $statusAjax.html('');
         $(that).val(''); // empty field
      },2000);
    }
  
  // Event handlers to show information when events are being emitted
    $('.autocomplete')
      .on('keydown', function (){  
      
        $statusKey.html('Waiting for more keystrokes... ');
      clearInterval(intervalId);             
      })
     
  
  // Display when the ajax request will happen (after user stops typing)
    // Exagerated value of 1.2 seconds for demo purposes, but in a real example would be better from 50ms to 200ms
  $('.autocomplete').on('keydown',
       _.debounce(make_ajax_request, 1300));
  });
```

 当用户输入并且发起`Ajax`请求的时候，`_.debounce`可以用来实现防抖，比如间隔2秒未检测到用户输入才发起请求。 

### 三、节流（Throttle）

 具体实现  --  使用定时器 

```javascript
var throttle = function(func, delay) {
    var timer = null;
    return function() {
        if (!timer) {
            timer = setTimeout(function() {
                func.apply(this,arguments);
                timer = null;
            },delay);
        }
    }
}
```

 另一种定时器写法： 

```javascript
let isAllow = true;
function throttle() {
    let fun = function() {
        if (!isAllow)
            return;
        let timer = setTimeout(() => {
            console.log("throttle");
            clearTimeout(timer);
            timer = null;
            isAllow = true;
        },1000);
    };
    fun()；
}
```

 二者的区别是后者使用了isAllow标志位来判断是否需要执行函数。 

 滚动条实例 

 一个十分常见的例子，当用户的鼠标滚动的时候，检查鼠标位置离底部的距离，当接近底部时，发起`Ajax`请求。 

```html
<h1>Infinite scrolling throttled</h1>
<div class="item color-1">Block 1</div>
<div class="item color-2">Block 2</div>
<div class="item color-3">Block 3</div>
<div class="item color-4">Block 4</div>
```

```css
body {
   background: #444444;
   color: white;
   font: 15px/1.51 system, -apple-system, ".SFNSText-Regular", "San Francisco", "Roboto", "Segoe UI", "Helvetica Neue", "Lucida Grande", sans-serif;
   margin:0 auto;
   max-width:600px;
   padding:20px;
}
.item {
  border:4px solid white;
  height:120px;
  width:100%;
  margin-bottom:50px;
  background:#333;
  padding:20px;
}



.color-1 { border-color: #9BFFBB}
.color-2 { border-color: #B9C6FF}
.color-3 { border-color: #FFA3D8}
.color-4 { border-color: #FF8E9B}
```

```javascript
// Very simple example.
// Probably you would want to use a 
// full-featured plugin like
// https://github.com/infinite-scroll/infinite-scroll/blob/master/jquery.infinitescroll.js
$(document).ready(function(){
  
  // Check every 200ms the scroll position
  $(document).on('scroll', _.throttle(function(){
    check_if_needs_more_content();
  }, 300));

  function check_if_needs_more_content() {     
    pixelsFromWindowBottomToBottom = 0 + $(document).height() - $(window).scrollTop() -$(window).height();
    
  // console.log($(document).height());
  // console.log($(window).scrollTop());
  // console.log($(window).height());
  //console.log(pixelsFromWindowBottomToBottom);
    
    
    if (pixelsFromWindowBottomToBottom < 200){
      // Here it would go an ajax request
      $('body').append($('.item').clone()); 
      
    }
  }
});
```



### requestAnimationFrame

 `requestAnimationFrame`是另一种限制函数执行次数的方式，可以认为与`_.throttle`类似，但是其拥有更高的准确度，因为其本身就是为了更好的精确度而生的原生API。 

**requestAnimationFrame的优点**

- 致力于60fps（16ms每帧），自己可以决定最好的渲染时间。
- 简单且标准的原生API，不太容易改变，减少维护成本。

**requestAnimationFrame的缺点**

- 需要手动开始或取消rAFs，`.debounce` 或 `.throttle`则不需要。
- 如果tab没有激活，则不会执行，即使触发 Ascroll, mouse 或者 keyboard 等事件也不行.
- 不支持 IE9, Opera Mini 和 老版本 Android.
- 不支持`node.js`，所以不能在服务端使用。

**requestAnimationFrame实例**

与`_.throttle`相比，同时设置 16ms， 相同的性能环境下，rAF 可以在更复杂的情况下拥有更好的结果。



### 四、结论

一般来说，如果你的JavaScript函数是直接绘制或者动画某些属性，那么可以使用`requestAnimationFrame`，在需要重新计算元素位置的地方使用。

需要发起`Ajax`请求，或者决定是否增加或删除一个类的时候，可以考虑使用`_.debounce` 或者`_.throttle` 。

debounce: 在事件被触发x秒后再执行回调，如果在这x秒内又被触发，则重新计时。

throttle: 规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。

requestAnimationFrame: 节流的另外一种选项，在函数重新计算或者渲染元素时，需要平滑过渡或者动画的时候使用。