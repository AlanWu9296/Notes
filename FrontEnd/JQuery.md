#  jQuery基础

jQuery库包含以下功能：

- HTML 元素选取
- HTML 元素操作
- CSS 操作
- HTML 事件函数
- JavaScript 特效和动画
- HTML DOM 遍历和修改
- AJAX
- Utilities

## 语法

**$(selector).action()**

```js
$(this).hide() - 隐藏当前元素

$("p").hide() - 隐藏所有 <p> 元素

$("p.test").hide() - 隐藏所有 class="test" 的 <p> 元素

$("#test").hide() - 隐藏所有 id="test" 的元素
```

**文档就绪事件**

```js
$(document).ready(function(){
 
   // 开始写 jQuery 代码...
 
});
```

## 选择器

jQuery 中所有选择器都以美元符号开头：$();基于已经存在的 [CSS 选择器](http://www.runoob.com/cssref/css-selectors.html)

## 事件

| 鼠标事件                                                     | 键盘事件                                                     | 表单事件                                                 | 文档/窗口事件                                            |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------------------------------- | -------------------------------------------------------- |
| [click](http://www.runoob.com/jquery/event-click.html)       | [keypress](http://www.runoob.com/jquery/event-keypress.html) | [submit](http://www.runoob.com/jquery/event-submit.html) | [load](http://www.runoob.com/jquery/event-load.html)     |
| [dblclick](http://www.runoob.com/jquery/event-dblclick.html) | [keydown](http://www.runoob.com/jquery/event-keydown.html)   | [change](http://www.runoob.com/jquery/event-change.html) | [resize](http://www.runoob.com/jquery/event-resize.html) |
| [mouseenter](http://www.runoob.com/jquery/event-mouseenter.html) | [keyup](http://www.runoob.com/jquery/event-keyup.html)       | [focus](http://www.runoob.com/jquery/event-focus.html)   | [scroll](http://www.runoob.com/jquery/event-scroll.html) |
| [mouseleave](http://www.runoob.com/jquery/event-mouseleave.html) |                                                              | [blur](http://www.runoob.com/jquery/event-blur.html)     | [unload](http://www.runoob.com/jquery/event-unload.html) |

# jQuery HTML

## 获取内容和属性

用于 DOM 操作的 jQuery 方法：

- text() - 设置或返回所选元素的文本内容
- html() - 设置或返回所选元素的内容（包括 HTML 标记）
- val() - 设置或返回表单字段的值
- attr() 方法用于获取属性值

```js
// get
$("#btn1").click(function(){
  alert("Text: " + $("#test").text());
});

$("#btn2").click(function(){
  alert("HTML: " + $("#test").html());
}); 

$("#btn1").click(function(){
  alert("值为: " + $("#test").val());
});

$("button").click(function(){
  alert($("#runoob").attr("href"));
});
```

```js
// set
$("#btn1").click(function(){
    $("#test1").text("Hello world!");
});
$("#btn2").click(function(){
    $("#test2").html("<b>Hello world!</b>");
});
$("#btn3").click(function(){
    $("#test3").val("RUNOOB");
});

$("button").click(function(){
    $("#runoob").attr({
        "href" : "http://www.runoob.com/jquery",
        "title" : "jQuery 教程"
    });
});

```

**回调函数**

回调函数有两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值。然后以函数新值返回您希望使用的字符串。

```js
$("#btn1").click(function(){
    $("#test1").text(function(i,origText){
        return "旧文本: " + origText + " 新文本: Hello world! (index: " + i + ")"; 
    });
});
 
$("#btn2").click(function(){
    $("#test2").html(function(i,origText){
        return "旧 html: " + origText + " 新 html: Hello <b>world!</b> (index: " + i + ")"; 
    });
});

$("button").click(function(){
    $("#runoob").attr({
        "href" : "http://www.runoob.com/jquery",
        "title" : "jQuery 教程"
    });
});
```

## 添加元素

添加新内容的四个 jQuery 方法：

- append() - 在被选元素的结尾插入内容
- prepend() - 在被选元素的开头插入内容
- after() - 在被选元素之后插入内容
- before() - 在被选元素之前插入内容

```js
function appendText()
{
    var txt1="<p>文本。</p>";              // 使用 HTML 标签创建文本
    var txt2=$("<p></p>").text("文本。");  // 使用 jQuery 创建文本
    var txt3=document.createElement("p");
    txt3.innerHTML="文本。";               // 使用 DOM 创建文本 text with DOM
    $("body").append(txt1,txt2,txt3);        // 追加新元素
}

function afterText()
{
    var txt1="<b>I </b>";                    // 使用 HTML 创建元素
    var txt2=$("<i></i>").text("love ");     // 使用 jQuery 创建元素
    var txt3=document.createElement("big");  // 使用 DOM 创建元素
    txt3.innerHTML="jQuery!";
    $("img").after(txt1,txt2,txt3);          // 在图片后添加文本
}
```

## 删除元素

使用以下两个 jQuery 方法：

- remove() - 删除被选元素（及其子元素）
- empty() - 从被选元素中删除子元素

```js
$("#div1").remove();

$("#div1").empty();
// remove() can take one filter parameter
$("p").remove(".italic");
```

## Css

- addClass() - 向被选元素添加一个或多个类
- removeClass() - 从被选元素删除一个或多个类
- toggleClass() - 对被选元素进行添加/删除类的切换操作
- css() - 设置或返回样式属性

## 尺寸

处理尺寸的重要方法：

- width()
- height()
- innerWidth()
- innerHeight()
- outerWidth()
- outerHeight()

![jQuery Dimensions](http://www.runoob.com/images/img_jquerydim.gif)

# 遍历 DOM

## 祖先

- parent() - parent() 方法返回被选元素的直接父元素
- parents() - parents() 方法返回被选元素的所有祖先元素，它一路向上直到文档的根元素 (<html>)
- parentsUntil() - parentsUntil() 方法返回介于两个给定元素之间的所有祖先元素

```js
$("span").parentsUntil("div");
```

## 后代

- children() - children() 方法返回被选元素的所有直接子元素。该方法只会向下一级对 DOM 树进行遍历
- find() - find() 方法返回被选元素的后代元素，一路向下直到最后一个后代

```js
// children() can take a filter parameter to only find the filtered elements 
$("div").children("p.1");
// find() also needs a filter parameter to find
$("div").find("span");
```

## 同胞

- siblings() - siblings() 方法返回被选元素的所有同胞元素, can take filter parameter
- next() - next() 方法返回被选元素的下一个同胞元素,该方法只返回一个元素
- nextAll() - nextAll() 方法返回被选元素的所有跟随的同胞元素
- nextUntil() - nextUntil() 方法返回介于两个给定参数之间的所有跟随的同胞元素
- prev()
- prevAll()
- prevUntil()

prev(), prevAll() 以及 prevUntil() 方法的工作方式与上面的方法类似，只不过方向相反而已：它们返回的是前面的同胞元素（在 DOM 树中沿着同胞之前元素遍历，而不是之后元素遍历）

## 过滤

- first()
- last()
- eq() - eq() 方法返回被选元素中带有指定索引号的元素。索引号从 0 开始，因此首个元素的索引号是 0 而不是 1
- filter() - 规定一个标准。不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回
- not() - 返回不匹配标准的所有元素

# jQuery 效果

通过 jQuery，可以把动作/方法链接在一起。

Chaining 允许我们在一条语句中运行多个 jQuery 方法（在相同的元素上）。

```js
$("#p1").css("color","red").slideUp(2000).slideDown(2000);

// or better for layout
$("#p1").css("color","red")
  .slideUp(2000)
  .slideDown(2000);
```



## 隐藏和显示

```js
$(selector).hide(speed,callback);
$(selector).show(speed,callback); 
// switch between hide() and show();
$(selector).toggle(speed,callback); 
```

## 淡入淡出

```js
$(selector).fadeIn(speed,callback);
$(selector).fadeOut(speed,callback);
// switch between fadeIn() and fadeOut()
$(selector).fadeToggle(speed,callback);
// fade to specific opacity
$(selector).fadeTo(speed,opacity,callback);
```

## 滑动

```js
$(selector).slideDown(speed,callback);
$(selector).slideUp(speed,callback);
$(selector).slideToggle(speed,callback);
```

## 动画

```js
$(selector).animate({params},speed,callback);

//multiple properties
$("button").click(function(){
  $("div").animate({
    left:'250px',
    opacity:'0.5',
    height:'150px',
    width:'150px'
  });
});

// use relative value so it can do infinite times!
$("button").click(function(){
  $("div").animate({
    left:'250px',
    height:'+=150px',
    width:'+=150px'
  });
});

// use default value in "show" ,"hide" and "toggle"
$("button").click(function(){
  $("div").animate({
    height:'toggle'
  });
});

// use animation like a queue in multiple steps!
$("button").click(function(){
  var div=$("div");
  div.animate({height:'300px',opacity:'0.4'},"slow");
  div.animate({width:'300px',opacity:'0.8'},"slow");
  div.animate({height:'100px',opacity:'0.4'},"slow");
  div.animate({width:'100px',opacity:'0.8'},"slow");
});
```

```js
$(selector).stop(stopAll,goToEnd);
//可选的 stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。
//可选的 goToEnd 参数规定是否立即完成当前动画。默认是 false。
$("#stop").click(function(){
  $("#panel").stop();
});
```

# jQuery AJAX

```js
// load() 方法从服务器加载数据，并把返回的数据放入被选元素中
$(selector).load(URL,data,callback);
```

可选的 callback 参数规定当 load() 方法完成后所要允许的回调函数。回调函数可以设置不同的参数：

- *responseTxt* - 包含调用成功时的结果内容
- *statusTXT* - 包含调用的状态
- *xhr* - 包含 XMLHttpRequest 对象

```js
// $.get() 方法通过 HTTP GET 请求从服务器上请求数据
$.get(URL,callback);

$("button").click(function(){
  $.get("demo_test.php",function(data,status){
    alert("数据: " + data + "\n状态: " + status);
  });
});
```

```js
// $.post() 方法通过 HTTP POST 请求向服务器提交数据
$.post(URL,data,callback);


$("button").click(function(){
    $.post("/try/ajax/demo_test_post.php",
    {
        name:"菜鸟教程",
        url:"http://www.runoob.com"
    },
        function(data,status){
        alert("数据: \n" + data + "\n状态: " + status);
    });
});
```

