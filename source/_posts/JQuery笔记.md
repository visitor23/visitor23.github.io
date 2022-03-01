---
title: JQuery笔记
date: 2022-02-20 02:26:00
author: 丁兆龙
img: 
top: false
mathjax: false
summary: JQuery笔记
categories: 

tags:

  - 前端
  - JQuery
---





## jQuery是什么

`jQuery`是一个快速，小巧，功能丰富的JavaScript库,使HTML文档遍历和操作，事件处理和动画等操作变得更加简单.**

**jQuery的所有功能都是通过JavaScript访问的.**

> **JQuery的特点**

* 轻量级
* 强大的选择器
* 出色的DOM操作的封装
* 可靠的事件处理机制
* 完善的Ajax
* 不污染顶级变量
* 出色的浏览器兼容性
* 链式操作方式
* 隐式迭代
* 行为层与结构层的分离
* 丰富的插件支持
* 完善的文档
* 开源



> 使用jQuery时，首先需要使用**脚本**标签将其**添加**到我们的HTML文档的标题：

```html
<!-- src设置jq的绝对或相对路径,用于引入jq,一般写在<head>标签中 -->
<script src="xxx/jquery.js"></script>
```



## jQuery入口函数

 在使用`HTML文档之前`等待HTML文档的`DOM节点准备就绪`后执行: 使用 `document(文档) `对象的 `ready`事件：

```javascript
$(document).ready(function() {
    // 在这写 jQuery 代码
    //类似于原生js的window.onload
});
```

`$ `用于访问`jQuery`。 从这里开始，代码访问document对象，并定义了当 `document `的 `ready `事件触发时要调用的函数。

**上述代码的简写方式：**

```javascript
$(function() {
   // 在这写 jQuery 代码
});
```



**`jQuery`入口函数与`window.onload`的区别：执行时机不同.**

* `jQuery`入口函数，是在文档加载完成后，就执行。即**DOM树加载完成后就可操作，不用等所有的外部资源都加载完成。**

- `Js`入口函数是在所有的文件资源加载完成后才执行。这些资源包括：**页面文档、外部的js文件、外部的css文件、图片等**。



## jQuery选择器

jQuery选择器以**美元符**和**圆括号**开头,允许对元素组或单个元素进行操作.

最基本的选择器包括id选择器,类选择器和元素选择器,它们根据相应的属性值来选择元素

```js
$("#test") // 选择id="test"的元素
$(".menu") // 选择class="menu"的所有元素
$("div")   // 选择所有<div>元素
```


![](http://r7lh60uut.hb-bkt.clouddn.com/JQ%E5%B8%B8%E7%94%A8%E9%80%89%E6%8B%A9%E5%99%A8.png)


## 常用方法

| 方法         | 说明                                                     |
| ------------ | -------------------------------------------------------- |
| attr()       | 获取或设置指定属性的值:当传入2个参数时修改属性为指定值   |
| removeAttr() | 删除元素的属性                                           |
| text()       | 获取或设置文本内容                                       |
| html()       | 获取或设置文本内容(包括HTML标记)                         |
| val()        | 获取或设置单表字段的值,例如文本框input和下拉列表select等 |
| prepend()    | 在被选元素中的开头插入内容                               |
| append()     | 在被选元素中的结尾插入内容                               |
| before()     | 在被选元素之前插入内容                                   |
| after()      | 在被选元素之后插入内容                                   |



## 操作CSS

jQuery 拥有若干进行 CSS 操作的方法：

- **`addClass()`** - 向被选元素添加一个或多个类
  要在 **addClass()** 方法中指定**多个类**, 只需使用**空格**分隔.下同. 例如$("div").addClass("class1 class2");

- **`removeClass()`** - 从被选元素删除一个或多个类

  如果**没有规定参数**,则从被选元素中**删除所有类**

- **`toggleClass()`** - 对被选元素进行添加/删除类的切换操作

  检查指定的类.不存在则**添加**,如果存在则**删除**.这就是所谓的切换效果

- **`css()`** - 设置或返回样式属性

  设置样式:$(selector).css(property,value)
  **property** ：规定 CSS **属性名称**，比如 "color"、"font-weight" 等
  **value**： 规定 CSS **属性的值**，比如 "red"、"bold" 等

**css()** 方法可以使用JSON语法设置多个CSS属性。

语法由`“属性”：“值”`对组成,以`,`分隔,并以`{}`包裹

```js
css({"property":"value","property":"value",...});
```



## 文档对象模型DOM

**DOM(Document Object Model)仅关注浏览器所载入的文档.**
**DOM将html文档视为由元素、属性和文本组成的一棵DOM树。**

可以将html文档中的**每个成分**视为一个**节点**，节点的特点如下：

- 整个**文档**是一个**文档节点**
- 每**个HTML标签**是一个**元素节点**
- 包含在HTML元素中的文本是**文本节点**
- 每个**HTML属性**是一个**属性节点**
- **注释**属于**注释节点**
- HTML文档中的节点彼此间都存在关系，如一张族谱。

**DOM**表示文档作为树结构，其中HTML元素是树中的相关节点。



### DOM遍历

jQuery 遍历，意为"移动"，用于根据其相对于其他元素的关系来"查找"（或选取）HTML 元素。以某项选择开始，并沿着这个选择移动，直到抵达期望的元素为止。

下图展示了一个家族树。通过` jQuery 遍历`，能够从当前元素开始，在家族树中向上移动（父节点），向下移动（子节点），水平移动（兄弟节点）。这种移动被称为对 DOM 进行遍历。



![](http://r7lh60uut.hb-bkt.clouddn.com/jQuery%E9%81%8D%E5%8E%86.png)

图示解析：

- `<div>` 元素是 `<ul>` 的父元素，同时是其中所有内容的祖先。
- `<ul>` 元素是 `<li>` 元素的父元素，同时是 `<div>` 的子元素
- 左边的` <li>` 元素是 `<span>` 的父元素，`<ul>` 的子元素，同时是 `<div>` 的后代。
- `<span>` 元素是` <li>` 的子元素，同时是 `<ul>` 和 `<div>` 的后代。
- 两个 `<li>` 元素是同胞（拥有相同的父元素）。
- 右边的` <li>` 元素是` <b>` 的父元素，`<ul>` 的子元素，同时是 `<div>` 的后代。
- `<b>` 元素是右边的 `<li>` 的子元素，同时是 `<ul>` 和` <div>` 的后代。



常用遍历方法：

**向上遍历** DOM 树：

- `parent()`  返回被选元素的直接父元素
- `parents()`  返回被选元素的所有祖先元素,直到文档的根元素`<html>`

**向下遍历** DOM 树

- `children()`  返回被选元素的所有直接子元素
- `find()`  返回被选元素的后代元素

**水平遍历**DOM树

- `siblings()`  返回被选元素的所有同胞元素
- `next()`  返回被选元素的下一个同胞元素
- `nextAll()`  返回被选元素的下面的所有同胞元素
- `prev()`  返回被选元素的上一个同胞元素
- `prevAll()`  返回被选元素的上面的所有同胞元素



 **eq() 方法**

返回被选元素中**指定索引**的元素.索引从0开始.

```js
//返回页面中的第3个div元素:
$("div").eq(2);
```



### 删除元素

如需删除元素和内容，一般可使用以下两个 jQuery 方法：

- **remove()** - 删除被选元素.包括其子元素
- **empty()** - 从被选元素中删除子元素



## 事件处理

指当 HTML 中发生某些事件时所调用的方法.通常写在`head`标签中

jQuery **事件处理**方法是 jQuery 中的**核心函数**。当用户**执行某些操作**即**发生事件**时,会执行处理函数.



### 常见事件

#### 鼠标事件

- **click**:  单击时发生
- **dblclick**:  双击元素时触发
- **mouseenter**:  当鼠标指针进入所选元素时触发
- **mouseleave**:  鼠标指针离开所选元素时触发
- **mouseover**:  当鼠标指针在所选元素上方悬停时触发

#### 键盘事件

- **keydown**:  当按下键盘按键时会触发
- **keyup**:  当键盘按键被释放时会触发
- **keypress**：当按钮按下并抬起同一个按键

#### 表单事件

- **submit**:  提交表单时触发
- **change**:  当表单元素的值发生改变时触发
- **focus**:  当表单元素获得焦点时触发
- **blur**:  当表单元素失去焦点时触发

#### 文件事件

- **ready**:  当DOM加载完成以后触发
- **resize**:  当浏览器窗口大小改变时触发
- **scroll**:  当用户在指定的元素中滚动滚动条时触发



例如，当用户输入时更改div的内容:

```html
<input type="text" id="name" />
<div id="msg"></div>
<script>
$("#name").keydown(function() {
  $("#msg").html($("#name").val());
});
</script>
```



### on()绑定事件

**on()** 方法在被选**元素及子元素上**添加**一个或多个**事件处理程序.

第1个参数是**事件名称**,当需要将处理函数绑定到**多个事件**上时,利用空格分隔事件名.

第2个参数是**处理函数**.

```js
$("p").on("click dblclick", function() {
  alert("单机或双击了p段落!");
});
```



### off()解绑事件

**off()方法通常用于移除通过on()方法添加的事件处理程序,参数为事件名.**

> **注意**：移除指定的事件处理程序时，事件名参数必须匹配 on() 方法传递的参数。

例如:解绑`div`元素通过on()方法绑定的事件:

```js
$("div").on("click", function() { 
  alert('Hi there!'); 
}); 
$("div").off("click");
```



### 事件对象

**事件对象event作为参数传递给事件处理函数,其中包含与该事件相关的属性和方法**

- **event.type**：获取事件的类型
- **event.pageX**和**event.pageY**：获取鼠标当前相对于页面的坐标（X和Y坐标）
- **event.preventDefault()方法**： 阻止默认行为
- **event.stopPropagation()方法**： 阻止事件冒泡
- **event.which**： 获取在鼠标单击时，单击的是鼠标的哪个键
- **event.data** 数据绑定事件时传入的任何数据
- **event.currentTarget**： 在事件冒泡过程中的当前DOM元素
- **event.result**： 包含由被指定事件触发的事件处理器返回的最后一个值



例如，处理`<a>`元素上的click事件,使点击时提醒鼠标位置,并阻止打开href属性中提供的链接：

```html
<a href="https://www.w3cschool.cn">点击我</a>
<script>
$("a").click(function(event) {
  alert(event.pageX);
  event.preventDefault();
});
</script>
```



### 触发事件

**trigger()方法可以在jQuery代码中以编程方式触发被选元素的指定事件**

例如,触发一个点击事件,而不需要用户实际点击一个元素：

```js
$("div").click(function() {
   alert("点击了div!");
});
$("div").trigger("click");
```

> **trigger()方法**不能用来模仿**本机浏览器事件**，比如点击一个文件文本框。 只能处理**jQuery事件系统中**的事件。



## 效果



### 常用效果

#### 显示/隐藏

jQuery有一些易于实现的效果来创建动画,可以使用 **hide()** 和 **show()** 方法来**隐藏**和**显示** HTML 元素,

使用**toggle()**方法**切换**元素的显示/隐藏状态.

**hide / show / toggle**方法可以带一个速度参数,以毫秒为单位指定动画速度

> hide / show / toggle 方法还有第二个可选参数可选，这是一个在动画完成后执行回调的方法


例如为toggle方法传入一个1000毫秒的参数：

```js
$(function() {
  $("p").click(function() {
    $("div").toggle(1000);
  });
});
```



#### 淡入/淡出

jQuery提供了 **fadeIn / fadeOut** 方法，它将一个元素淡入和淡出显示

**fadeToggle**()方法可以在淡入淡出中进行**切换**。

实例：

```js
$(function() {
  $("p").click(function() {
    $("div").fadeToggle(1000);
  });
});
```

和**toggle**()一样，**fadeToggle**()也具有两个可选参数：速度和回调函数。

> 用于**淡入/淡出**的另一种方法是**fadeTo()**，它将淡入/淡出到给定的**不透明度**（0和1之间的值）。 例如：$("div").fadeTo(1500,0.7);



#### 向上/向下滑动

**slideUp()** 和 **slideDown()** 方法用于在元素上创建滑动效果,

**slideToggle()** 方法在**滑动效果**之间**切换**，也有两个可选参数：**速度**和**回调函数**。

例如：

```js
$(function() {
  $("p").click(function() {
    $("div").slideToggle(500);
  });
});
```



### 动画

jQuery **animate()** 方法用于创建**自定义动画**

> **语法：$(selector).animate({params},speed,callback); **

- **params**--必需，定义形成动画的 CSS 属性。属性定义为**JSON格式**的**参数**（"key":"value"）
- **speed**--规定效果的**时长**。它可以取以下值：**"slow"、"fast"** 或**毫秒**
- **callback** --动画完成后所执行的**函数名称**

例如，以下代码将div的width属性在1秒内改变到250px：

```js
$("div").click(function() {
  $("div").animate({width: '250px'}, 1000);
});
```

请注意提供CSS参数的JSON格式。 在处理CSS属性时，JSON语法也被用于以前的模块。

> 可以使用上述语法对任何CSS属性进行动画处理，但当与**animate()** 方法一起使用时，所有**属性名称**都必须是**camel-cased**（小驼峰命名法）.
> 将需要编写paddingLeft而不是padding-left，marginRight，而不是margin-right等



#### 多个动画设置

多个属性可以通过用**逗号**分隔来同时**动画**化

例如：

```js
$("div").animate({
  width: '250px',
  height: '250px'
}, 1000);
```

也可以定义**相对值**（该值相对于**元素的当前值**）。 通过将 += 或 -= 放在值的前面完成：

```js
$("div").animate({
  width: '+=250px',
  height: '+=250px'
}, 1000);
```

> 要在动画完成之前停止动画，jQuery提供了**stop()**方法。



#### 停止动画stop()

jQuery `stop()`方法用于在动画或效果完成前对它们进行停止

`stop()`方法适用于所有 jQuery 效果函数，包括滑动、淡入淡出和自定义动画

> 语法：$(selector).stop(stopAll,goToEnd);

- **` stopAll `**--`可选`，规定是否应该`清除动画队列`。默认是 `false`，即仅停止活动的动画，允许任何排入队列的动画向后执行。
- **`goToEnd`** --规定是否立即完成`当前动画`。默认是 false。

因此，默认地，stop() 会清除在被选元素上指定的当前动画。




#### 动画队列

默认情况下，jQuery带有**动画**的**队列**功能。

这意味着如果你写了多个**animate()**，则jQuery会为这些方法创建一个**“内部”**队列。然后逐个运行动画。

例如：

```
var div = $("div");
div.animate({opacity: 1});
div.animate({height: '+=100px', width: '+=100px', top: '+=100px'}, 500);
div.animate({height: '-=100px', width: '-=100px', left: '+=100px'}, 500);
div.animate({height: '+=100px', width: '+=100px', top: '-=100px'}, 500);
div.animate({height: '-=100px', width: '-=100px', left: '-=100px'}, 500);
div.animate({opacity: 0.5});
```

以上的animate() 方法将一个接一个地运行。


![](http://r7lh60uut.hb-bkt.clouddn.com/%E5%8A%A8%E7%94%BB%E9%98%9F%E5%88%97.gif)


要**操纵元素**的**位置**，需要将元素的CSS位置属性设置为 **relative，fixed** 或 **absolute**。

> **animate()** 方法，就像 **hide / show / fade / slide** 方法一样，可以使用可选的**回调函数**作为其参数，该参数在当前效果完成后执行。





#### 动画的callback回调

`Callback 函数`在当前动画 100% 完成之后执行。

许多jQuery函数涉及动画.这些函数也许会将 `speed `或 `duration `作为可选参数

例子：$("p").hide("slow")

`speed `或 `duration `参数可以设置许多不同的值，比如 "slow", "fast", "normal" 或毫秒数

例如：

```html
<button>隐藏</button>
<p>我们段落内容，点击“隐藏”按钮我就会消失</p>
```

```js
$(document).ready(function(){
  $("button").click(function(){
    $("p").hide("slow",function(){
      alert("段落现在被隐藏了");
    });
  });
});
```

结果：



![](http://r7lh60uut.hb-bkt.clouddn.com/%E5%8A%A8%E7%94%BB%E7%9A%84callback%E5%9B%9E%E8%B0%83.png)


上面实例在`隐藏`效果`完全实现后`回调函数，也就是`警告框`会在隐藏效果完成后才会弹出：



### 下拉式菜单

```html
<div class="menu">
  <div id="item">下拉菜单</div>
  <div id="submenu">
    <a href="#">选项1</a>
    <a href="#">选项2</a>
    <a href="#">选项3</a>
  </div>
</div>
```

```js
$("#item").click(function() {
  $("#submenu").slideToggle(500);
}); 
```

上面的代码处理 id="item" 元素的点击事件，并在500毫秒内打开/关闭子菜单。



