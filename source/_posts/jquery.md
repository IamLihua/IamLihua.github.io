---
title: jquery
date: 2023-03-22 22:22:12
tags: javascript
---

# jquery笔记

## 视频教程

【尚硅谷最新版JavaWeb全套教程,java web零基础入门完整版】 [【尚硅谷最新版JavaWeb全套教程】](https://www.bilibili.com/video/BV1Y7411K7zz/?p=63&share_source=copy_web&vd_source=dd2f1030c390db245979ca6615985682)

这是一篇围绕这个视频教程和jquery文档的笔记

## 关于$

注意下面这个程序导入了"../script/jquery-1.7.2.js"，请确保工作目录下有这个文件

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
	<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
	<script type="text/javascript">
		// window.onload = function () {
		// 	var btnObj = document.getElementById("btnId");
		// 	// alert(btnObj);//[object HTMLButtonElement]   ====>>>  dom对象
		// 	btnObj.onclick = function () {
		// 		alert("js 原生的单击事件");
		// 	}
		// }

		$(function () { // 表示页面加载完成 之后，相当 window.onload = function () {}
			var $btnObj = $("#btnId"); // 表示按id查询标签对象，返回的是一个jquery对象
			$btnObj.click(function () { // 绑定单击事件
				alert("jQuery 的单击事件");
			});
		});

	</script>



</head>
<body>
	<button id="btnId">SayHello</button>
</body>
</html>
```

$()里面如果放一个函数，意思是这个函数要在window.onload时执行

四种用法见下面

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
<script type="text/javascript">

	//核心函数的4个作用
    $(function () {
        // alert("页面加载完成之后，自动调用");

        $("    <div>" +
            "        <span>div-span1</span>" +
            "        <span>div-span2</span>" +
            "    </div>").appendTo("body");

        // alert($("button").length);

        var btnObj = document.getElementById("btn01");
        // alert(btnObj);
        // alert( $(btnObj) );

        // alert( $("<h1></h1>") );
        alert($("button"));

    });
	//传入参数为[函数]时：在文档加载完成后执行这个函数
	//传入参数为[HTML字符串]时：根据这个字符串创建元素节点对象
	//传入参数为[选择器字符串]时：根据这个字符串查找元素节点对象，和queryselsctorAll类似，注意它返回的是一个Dom对象数组
	//传入参数为[DOM对象]时：将DOM对象包装为jQuery对象返回

</script>
</head>
<body>
    <button id="btn01">按钮1</button>
    <button>按钮2</button>
    <button>按钮3</button>
</body>
</html>
```

### dom对象与jquery对象

感觉后者是前者包装后的数组

互相转化的方法：

- $(dom)===>jquery
- jquery[index]===>dom

## 层级选择器

- ancestor descendant 后代选择器 ：在给定的祖先元素下匹配所有的后代元素 
- parent > child 子元素选择器：在给定的父元素下匹配所有的子元素 
- prev + next 相邻元素选择器：匹配所有紧接在 prev 元素后的 next 元素 
- prev ~ sibings 之后的兄弟元素选择器：匹配 prev 元素之后的所有 siblings

## 过滤选择器

即在选择到的基础上进行筛选

### 基本过滤器

- :first 获取第一个元素
- :last 获取最后个元素 
- :not(selector) 去除所有与给定选择器匹配的元素 
- :even 匹配所有索引值为偶数的元素，从 0 开始计数 
- :odd 匹配所有索引值为奇数的元素，从 0 开始计数 
- :eq(index) 匹配一个给定索引值的元素 
- :gt(index) 匹配所有大于给定索引值的元素 
- :lt(index) 匹配所有小于给定索引值的元素 
- :header 匹配如 h1, h2, h3 之类的标题元素 
- :animated 匹配所有正在执行动画效果的元素

### 内容过滤器

- :contains(text) 匹配包含给定文本的元素 
- :empty 匹配所有不包含子元素或者文本的空元素 
- :parent 匹配含有子元素或者文本的元素 
- :has(selector) 匹配含有选择器所匹配的元素的元素

### 属性过滤器

- [attribute] 匹配包含给定属性的元素。
- [attribute=value] 匹配给定的属性是某个特定值的元素
- [attribute!=value] 匹配所有不含有指定的属性，或者属性不等于特定值的元素。
- [attribute^=value] 匹配给定的属性是以某些值开始的元素
- [attribute$=value] 匹配给定的属性是以某些值结尾的元素
- [attribute*=value] 匹配给定的属性是以包含某些值的元素

HTML 代码:

```html
<input type="checkbox" name="newsletter" value="Hot Fuzz" />
<input type="checkbox" name="newsletter" value="Cold Fusion" />
<input type="checkbox" name="accept" value="Evil Plans" />
```

jQuery 代码:

```js
$("input[name='newsletter']").attr("checked", true);
```

结果:

```js
[ <input type="checkbox" name="newsletter" value="Hot Fuzz" checked="true" />, <input type="checkbox" name="newsletter" value="Cold Fusion" checked="true" /> ]
```

### 表单过滤器

- :input 匹配所有 input, textarea, select 和 button 元素
- :text 匹配所有 文本输入框
- :password 匹配所有的密码输入框
- :radio 匹配所有的单选框
- :checkbox 匹配所有的复选框
- :submit 匹配所有提交按钮
- :image 匹配所有 img 标签
- :reset 匹配所有重置按钮
- :button 匹配所有 input type=button <button>按钮
- :file 匹配所有 input type=file 文件上传
- :hidden 匹配所有不可见元素 display:none 

这个好像没啥特别的，就是专门用来匹配表单里面的，至于不用它能不能匹配到，这个还不太清楚

### 表单对象属性过滤器

跟之前的过滤器一样

- :enabled 匹配所有可用元素 
- :disabled 匹配所有不可用元素

## 元素筛选

和过滤选择其实差不多，这里略

## 一个简单的区分

### selector1,selector2,selectorN

> 返回值:Array<Element(s)>

#### 示例

##### HTML 代码:

```html
<div>div</div>
<p class="myClass">p class="myClass"</p>
<span>span</span>
<p class="notMyClass">p class="notMyClass"</p>
```

##### jQuery 代码:

```js
$("div,span,p.myClass")
```

##### 结果:

```html
[ <div>div</div>, <p class="myClass">p class="myClass"</p>, <span>span</span> ]
```

### ancestor descendant

返回值:Array<Element(s)>

在给定的祖先元素下匹配所有的后代元素

#### 示例

找到表单中所有的 input 元素

##### HTML 代码:

```html
<form>
  <label>Name:</label>
  <input name="name" />
  <fieldset>
      <label>Newsletter:</label>
      <input name="newsletter" />
 </fieldset>
</form>
<input name="none" />
```

##### jQuery 代码:

```js
$("form input")
```

##### 结果:

```html
[ <input name="name" />, <input name="newsletter" /> ]
```

### parent > child

返回值:Array<Element(s)>

在给定的父元素下匹配所有的子元素

#### 示例

匹配表单中所有的子级input元素。

##### HTML 代码:

```html
<form>
  <label>Name:</label>
  <input name="name" />
  <fieldset>
      <label>Newsletter:</label>
      <input name="newsletter" />
 </fieldset>
</form>
<input name="none" />
```

##### jQuery 代码:

```js
$("form > input")
```

##### 结果:

```html
[ <input name="name" /> ]
```

### prev + next

返回值:Array<Element(s)>

匹配所有紧接在 prev 元素后的 next 元素

#### 示例

匹配所有跟在 label 后面的 input 元素

##### HTML 代码:

```html
<form>
  <label>Name:</label>
  <input name="name" />
  <fieldset>
      <label>Newsletter:</label>
      <input name="newsletter" />
 </fieldset>
</form>
<input name="none" />
```

##### jQuery 代码:

```js
$("label + input")
```

##### 结果:

```html
[ <input name="name" />, <input name="newsletter" /> ]
```

### prev ~ siblings

返回值:Array<Element(s)>

匹配 prev 元素之后的所有 siblings 元素

#### 示例

找到所有与表单同辈的 input 元素

##### HTML 代码:

```html
<form>
  <label>Name:</label>
  <input name="name" />
  <fieldset>
      <label>Newsletter:</label>
      <input name="newsletter" />
 </fieldset>
</form>
<input name="none" />
```

##### jQuery 代码:

```js
$("form ~ input")
```

##### 结果:

```html
[ <input name="none" /> ]
```

### :first

返回值:jQuery

获取第一个元素

#### 示例

获取匹配的第一个元素

##### HTML 代码:

```html
<ul>
    <li>list item 1</li>
    <li>list item 2</li>
    <li>list item 3</li>
    <li>list item 4</li>
    <li>list item 5</li>
</ul>
```

##### jQuery 代码:

```js
$('li:first');
```

##### 结果:

```html
[ <li>list item 1</li> ]
```

### :nth-child

返回值:Array<Element(s)>

匹配其父元素下的第N个子或奇偶元素

':eq(index)'  只匹配一个元素，而这个将为每一个父元素匹配子元素。:nth-child**从1开始的**，而:eq()是从0算起的！可以使用:<br>nth-child(even)<br>:nth-child(odd)<br>:nth-child(3n)<br>:nth-child(2)<br>:nth-child(3n+1)<br>:nth-child(3n+2)

#### 示例

在每个 ul 查找第 2 个li

##### HTML 代码:

```html
<ul>
  <li>John</li>
  <li>Karl</li>
  <li>Brandon</li>
</ul>
<ul>
  <li>Glen</li>
  <li>Tane</li>
  <li>Ralph</li>
</ul>
```

##### jQuery 代码:

```js
$("ul li:nth-child(2)")
```

##### 结果:

```html
[ <li>Karl</li>,   <li>Tane</li> ]
```

## jQuery 的属性操作

html() 	它可以设置和获取起始标签和结束标签中的内容。 跟 dom 属性 innerHTML 一样。
text() 	它可以设置和获取起始标签和结束标签中的文本。 跟 dom 属性 innerText 一样。
val() 	它可以设置和获取表单项的 value 属性值。 跟 dom 属性 value 一

```html
<!DOCTYPE html>
<html lang="zh_CN">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" src="script/jquery-1.7.2.js"></script>
    <script type="text/javascript">

        $(function () {
            // 不传参数，是获取，传递参数是设置
            // alert( $("div").html() );// 获取
            // $("div").html("<h1>我是div中的标题 1</h1>");// 设置

            // 不传参数，是获取，传递参数是设置
            // alert( $("div").text() ); // 获取
            // $("div").text("<h1>我是div中的标题 1</h1>"); // 设置

            // 不传参数，是获取，传递参数是设置
            $("button").click(function () {
                alert($("#username").val());//获取
                $("#username").val("超级程序猿");// 设置
            });


        });

    </script>
</head>
<body>
    <div>我是div标签 <span>我是div中的span</span></div>
    <input type="text" name="username" id="username" />
    <button>操作输入框</button>
</body>
</html>
```

attr() 可以设置和获取属性的值，不推荐操作 checked、readOnly、selected、disabled 等等
attr 方法还可以操作非标准的属性。比如自定义属性：abc,bbj
prop() 可以设置和获取属性的值,只推荐操作 checked、readOnly、selected、disabled 等

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="../../script/jquery-1.7.2.js"></script>
<script type="text/javascript">
	
	$(function(){
		// 给全选绑定单击事件
		$("#checkedAllBtn").click(function () {
			$(":checkbox").prop("checked",true);
		});

		// 给全不选绑定单击事件
		$("#checkedNoBtn").click(function () {
			$(":checkbox").prop("checked",false);
		});

		// 反选单击事件
		$("#checkedRevBtn").click(function () {
			// 查询全部的球类的复选框
			$(":checkbox[name='items']").each(function () {
				// 在each遍历的function函数中，有一个this对象。这个this对象是当前正在遍历到的dom对象
				this.checked = !this.checked;
			});

			// 要检查 是否满选
			// 获取全部的球类个数
			var allCount = $(":checkbox[name='items']").length;
			// 再获取选中的球类个数
			var checkedCount = $(":checkbox[name='items']:checked").length;

			// if (allCount == checkedCount) {
			// 	$("#checkedAllBox").prop("checked",true);
			// } else {
			// 	$("#checkedAllBox").prop("checked",false);
			// }

			$("#checkedAllBox").prop("checked",allCount == checkedCount);

		});

		// 【提交】按钮单击事件
		$("#sendBtn").click(function () {
			// 获取选中的球类的复选框
			$(":checkbox[name='items']:checked").each(function () {
				alert(this.value);
			});
		});

		// 给【全选/全不选】绑定单击事件
		$("#checkedAllBox").click(function () {

			// 在事件的function函数中，有一个this对象，这个this对象是当前正在响应事件的dom对象
			// alert(this.checked);

			$(":checkbox[name='items']").prop("checked",this.checked);
		});

		// 给全部球类绑定单击事件
		$(":checkbox[name='items']").click(function () {
			// 要检查 是否满选
			// 获取全部的球类个数
			var allCount = $(":checkbox[name='items']").length;
			// 再获取选中的球类个数
			var checkedCount = $(":checkbox[name='items']:checked").length;

			$("#checkedAllBox").prop("checked",allCount == checkedCount);
		});

	});
	
</script>
</head>
<body>

	<form method="post" action="">
	
		你爱好的运动是？<input type="checkbox" id="checkedAllBox" />全选/全不选 
		
		<br />
		<input type="checkbox" name="items" value="足球" />足球
		<input type="checkbox" name="items" value="篮球" />篮球
		<input type="checkbox" name="items" value="羽毛球" />羽毛球
		<input type="checkbox" name="items" value="乒乓球" />乒乓球
		<br />
		<input type="button" id="checkedAllBtn" value="全　选" />
		<input type="button" id="checkedNoBtn" value="全不选" />
		<input type="button" id="checkedRevBtn" value="反　选" />
		<input type="button" id="sendBtn" value="提　交" />
	</form>

</body>
</html>
```

## DOM 的增删改

内部插入：
appendTo() a.appendTo(b) 把 a 插入到 b 子元素末尾，成为最后一个子元素
prependTo() a.prependTo(b) 把 a 插到 b 所有子元素前面，成为第一个子元素
外部插入：
insertAfter() a.insertAfter(b) 得到 ba
insertBefore() a.insertBefore(b) 得到 ab
替换:
replaceWith() a.replaceWith(b) 用 b 替换掉 a
replaceAll() a.replaceAll(b) 用 a 替换掉所有 b
删除：
remove() a.remove(); 删除 a 标签
empty() a.empty(); 清空 a 标签里的内容

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
	<style type="text/css">
		select {
			width: 100px;
			height: 140px;
		}
		
		div {
			width: 130px;
			float: left;
			text-align: center;
		}
	</style>
	<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
	<script type="text/javascript">	
		$(function(){
			//选中添加到右边
			$("button:eq(0)").click(function(){
				$("select[name=sel01] :selected").each(function(){
					//alert(this);
					$(this).appendTo("select[name=sel02]");
				});
			});
			
			//全部添加到右边
			$("button:eq(1)").click(function(){
				$("select[name=sel01] option").each(function(){
					//alert(this);
					$(this).appendTo("select[name=sel02]");
				});
			});
			
			//选中删除到左边
			$("button:eq(2)").click(function(){
				$("select[name=sel02] :selected").each(function(){
					//alert(this);
					$(this).appendTo("select[name=sel01]");
				});
			});
			//全部删除到左边
			$("button:eq(3)").click(function(){
				$("select[name=sel02] option").each(function(){
					//alert(this);
					$(this).appendTo("select[name=sel01]");
				});
			});
		});
	
	</script>
</head>
<body>

	<div id="left">
		<select multiple="multiple" name="sel01">
			<option value="opt01">选项1</option>
			<option value="opt02">选项2</option>
			<option value="opt03">选项3</option>
			<option value="opt04">选项4</option>
			<option value="opt05">选项5</option>
			<option value="opt06">选项6</option>
			<option value="opt07">选项7</option>
			<option value="opt08">选项8</option>
		</select>
		
		<button>选中添加到右边</button>
		<button>全部添加到右边</button>
	</div>
	<div id="rigth">
		<select multiple="multiple" name="sel02">
		</select>
		<button>选中删除到左边</button>
		<button>全部删除到左边</button>
	</div>

</body>
</html>
```

## 设置css

就像这样

```html
$("p").css({ color: "#ff0011", background: "blue" });
```

## 事件的冒泡

什么是事件的冒泡？
事件的冒泡是指，父子元素同时监听同一个事件。当触发子元素的事件的时候，同一个事件也被传递到了父元素的事件里去
响应。
那么如何阻止事件冒泡呢？
在子元素事件函数体内，return false; 可以阻止事件的冒泡传递。

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<style type="text/css">
			*{
				margin: 0;
				padding: 0;
			}
			body{
				font-size: 13px;
				line-height: 130%;
				padding: 60px;
			}
			#content{
				width: 220px;
				border: 1px solid #0050D0;
				background: #96E555;
			}
			span{
				width: 200px;
				margin: 10px;
				background: #666666;
				cursor: pointer;
				color: white;
				display: block;
			}
			p{
				width: 200px;
				background: #888;
				color: white;
				height: 16px;
			}
		</style>
		<script type="text/javascript" src="jquery-1.7.2.js"></script>
		<script type="text/javascript">
			$(function(){
				
				//冒泡就是事件的向上传导，子元素的事件被触发，父元素的响应事件也会触发
				//解决冒泡问题：return false;
				
				//给span绑定一个单击响应函数
				$("span").click(function(){
					alert("我是span的单击响应函数");
					return false;
				});
				
				//给id为content的div绑定一个单击响应函数
				$("#content").click(function(){
					alert("我是div的单击响应函数");
					return false;
				});
				
				//给body绑定一个单击响应函数
				$("body").click(function(){
					//alert("我是body的单击响应函数");
				});
				
				//取消默认行为
				/* $("a").click(function(){
					return false;
				}) */
				
			})
		</script>
	</head>
	<body>
		<div id="content">
			外层div元素
			<span>内层span元素</span>
			外层div元素
		</div>
		
		<div id="msg"></div>	
		
		<br><br>
		<a href="http://www.hao123.com" onclick="return false;">WWW.HAO123.COM</a>	
	</body>
</html>

```

## 事件对象

### 1.原生 javascript 获取事件对象

```js
window.onload = function () {
	document.getElementById("areaDiv").onclick = function (event) {
		console.log(event);
	}
}
```

### 2.jQuery 代码获取事件对象

```js
$(function () {
	$("#areaDiv").click(function (event) {
		console.log(event);
	});
});
```

### 3.使用 bind 同时对多个事件绑定同一个函数。怎么获取当前操作是什么事件。

```js
$("#areaDiv").bind("mouseover mouseout",function (event) {
	if (event.type == "mouseover") {
		console.log("鼠标移入");
	} else if (event.type == "mouseout") {
		console.log("鼠标移出");
	}
});
```

