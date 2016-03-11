# 前端知识点

- `HTML&CSS`：语义化、布局、盒子模型、选择器优先级、flexbox、栅格化、浏览器渲染原理、CSS优化、SEO、移动端（兼容、bug、坑）多屏
- `Javascript`：数据类型、运算符优先级、继承、作用域、Function、DOM、BOM、闭包、异步、原型链、事件、正则、Ajax、路由、模块化、内存泄露、this上下文、按需加载、数组操作、排序、去重、DomReady、ES5方法
- `原理`：angular、vue、react、promise、webview、react-native、HTTP协议（状态码等）
- `性能`：chrome timeline、profile、nodejs等
- `测试`：mocha/jasmine/spy
- `语言`：php、c#、nodejs、java、go、python
- `部署`: linux、docker、nginx、haproxy、lua、LVS


# HTML

## 语义化

书写html尽量使用具有语义化信息的标签，例如header、nav等；有利于DOM结构组织、利于搜索引擎(SEO）理解，提升使用网络获取信息的检验。

 > 自定义标签（元素）方法
 
 - document.createElement
 - [document.registerElement](http://blog.bingo929.com/custom-elements-html-web-components.html)
 - `*` 通配符方法: `body > * p {}`
 
 ```
 <!--[if lt IE 9]>
<style>
body > * .section {
    display: block;
    padding: .5em;
    border-left: 3px solid #ddd;
    color: #999;
}
</style>
<![endif]-->
<style>
section .section {
    display: block;
    padding: .5em;
    border-left: 3px solid #ddd;
    color: #999;
}
</style>
```

- 使用命名空间

```
修改<html>标签处的命名空间
使用类似<html5:section><html5:/section>标签
使用如下选择器名称进行控制：html5\:section {}
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:html5="http://www.w3.org/1999/xhtml">
    <body>
        <html5:section>...</html5:section>
    </body>
</html>

.section, section, html5\:section {
    display: block;
    padding: .5em;
    border-left: 3px solid #ddd;
    color: #999;
}
```

- 使用框架：react/vue/angular

## 布局

> 固定布局、流式布局、响应式布局、弹性布局、多列布局、栅格化布局

### 流式布局：三列布局自适应布局

```
  // Dom 结构
  <div class="left></div>
  <div class="right"></div>
  <div class="center"></div>
  
  // style
  .left { float: left; width: 100px; }
  .right { float: right: width: 100px;}
  .center { }
```

品字布局：

```
	// Dom 结构
	<div class="top"></div>
	<div class="left"></div>
	<div class="right"></div>
	
	// style
	.top { width: 100%; clear: both; }
	.left { float: left; width: 50%; }
	.right { margin-left: 50%; }
```

### 响应式布局：Media Query

### 弹性布局：Flex box 
>（IE10+兼容）微信、UC不支持（可使用display:inline-box兼容），优势：设置等高对齐

http://www.w3cplus.com/css3/a-guide-to-flexbox-new.html

**兼容性**

http://www.w3cplus.com/css3/harnessing-flexbox-for-todays-web-apps.html

```
- PC端：
1、无前缀：Chrome 29+, Firefox 28+, IE 11+, Opera 17+
2、需要前缀：Chrome 21+, Safari 6.1+, Opera 15+需要前缀-webkit-
提示：旧版本的Firefox（22-27）支持除了flex-wrap和flex-flow之外的新语法。Opera (12.1+ - 17+)使用flex可以没有私有前缀，但是中间的15和16版本需要私有前缀。

- 支持中间语法的浏览器
PC端和移动端：IE10（使用-ms-私有前缀）。

- 支持旧语法的浏览器
所有PC端和移动端的浏览器都需要加-webkit-的私有前缀（除了firefox需要使用-moz-前缀）。

PC端：Firefox 2 – 21, Chrome 4 – 20, Safari 3.1 – 6
移动端Android 2.1 – 4.3, iOS 3.2 – 6.1, UC browser 9.9 on Android, BlackBerry 7

- 不支持的浏览器
PC端：IE9和Opera12以下
移动端Opera Mini
```

### 多列布局

#### 相关属性 （IE10+）

```
column-width: 200px; // 每列宽度
column-count: 3; // 每行显示几列
column-gap: 1.5em; // 列间距

// 列样式
column-rule:3px outset #ff00ff;
column-rule: [width] [style] [color]; 
column-rule-style
column-rule-width
column-rule-color

// 单列跨度：代表该列占整行
column-span: all； or none

// 截断
break-inside: avoid; 
break-before: avoid; 
```

### 栅格化布局（网格布局）

#### 相关属性 IE10+部分支持，其它浏览器基本不支持，可以借助JS实现（憋足）

```
display: -ms-grid;
-ms-grid-columns: 200px 20px auto 20px 250px; 
-ms-grid-rows: auto 1fr;
-ms-grid-column: 1; 
-ms-grid-row: 2;
```

## 选择器优先级

```
 1、!import
 2、inline style
 3、ID
 4、CLASS
 5、元素（标签）
 6、子元素 > （IE7+）
 7、兄弟选择 + （IE7+）
 8、子选择 ～ （IE7+）
 9、属性
 10、伪类
 11、动态伪类
```

## CSS 渲染原理

- 通过HTML Parse -> DOM Tree -> attachment
- style sheet -> css parse -> attachment
- attachment -> layout -> Render Tree -> Painting -> display
- 如果DOM变化不影响几何属性，则只会发生repaint（而不是reflow

```
由于每次reflow都会产生计算消耗，大多数浏览器都通过队列化修改（queuing changes）并批量执行来优化重排过程。  但是，获取布局信息的（返回最新的布局信息）操作会导致队列被强制刷新（浏览器马上执行渲染队列中的‘the pending changes’并触发重排以返回正确值），
比如：
offset(Top|Left|Width|Height)
scroll(Top|Left|Width|Height)
client(Top|Left|Width|Height)
getComputedStyle() (IE)
```

```
repaint(重绘) ，repaint发生更改时，元素的外观被改变，且在没有改变布局的情况下发生，如改变outline,visibility,background color，不会影响到dom结构渲染。

reflow(渲染)，与repaint区别就是他会影响到dom的结构渲染，同时他会触发repaint，他会改变他本身与所有父辈元素(祖先)，这种开销是非常昂贵的，导致性能下降是必然的，页面元素越多效果越明显。

何时发生：

1. DOM元素的添加、修改（内容）、删除( Reflow + Repaint)
2. 仅修改DOM元素的字体颜色（只有Repaint，因为不需要调整布局）
3. 应用新的样式或者修改任何影响元素外观的属性
4. Resize浏览器窗口、滚动页面
5. 读取元素的某些属性（offsetLeft、offsetTop、offsetHeight、offsetWidth、 scrollTop/Left/Width/Height、clientTop/Left/Width/Height、 getComputedStyle()、currentStyle(in IE)) 

如何避免：

1. 先将元素从document中删除，完成修改后再把元素放回原来的位置
2. 将元素的display设置为”none”，完成修改后再把display修改为原来的值
3. 如果需要创建多个DOM节点，可以使用DocumentFragment创建完后一次性的加入document 　　

var fragment = document.createDocumentFragment();
fragment.appendChild(document.createTextNode('keenboy test 111'));
fragment.appendChild(document.createElement('br'));
fragment.appendChild(document.createTextNode('keenboy test 222'));
document.body.appendChild(fragment);

4. 集中修改样式 
　　4.1尽可能少的修改元素style上的属性 
　　4.2尽量通过修改className来修改样式
　　4.3通过cssText属性来设置样式值
　　　　element.style.width=”80px”;  //reflow
　　　　element.style.height=”90px”; //reflow
　　　　element.style.border=”solid 1px red”; //reflow
　　　　以上就产生多次reflow，调用的越多产生就越多
　　　　element.style.cssText=”width:80px;height:80px;border:solid 1px red;”; //reflow　
　　4.4缓存Layout属性值 
　　　　var left=elem.offsetLeft; 多次使用left也就产生一次reflow
　　4.5设置元素的position为absolute或fixed
　　　　元素脱离标准流，也从DOM树结构中脱离出来，在需要reflow时只需要reflow自身与下级元素
　　4.6尽量不要用table布局
　　　　table元素一旦触发reflow就会导致table里所有的其它元素 reflow。在适合用table的场合，可以设置table-layout为auto或fixed，这样可以让table一行一行的渲染，这种做法也是为了限制reflow的影响范围
　　4.7避免使用expression,他会每次调用都会重新计算一遍(包括加载页面)
```

## 移动端

### 理论

> 尺寸

IOS：

- iphone 4/4s : 640*960
- iphone 5/5s/5c : 640*1136
- iphone 6: 750*1334
- iphone 6plus: 1080 * 1920/1125 * 2001/1242 * 2208

Android：

- 480 * 800
- 720 * 1280
- 1080 * 1920	 

> 屏幕： 设备像素比 = 物理像素 / 设备独立像素 // 在某一方向上，x方向或者y方向

- 物理像素
- 设备独立像素：设备宽高为375×667，可以理解为设备独立像素(或css像素）
- 设备像素比 dpr

```
在javascript中，可以通过window.devicePixelRatio获取到当前设备的dpr。

IE以及FireFox压根不支持。可能接下来的版本会支持。
Opera桌面浏览器时，即使是视网膜设备，返回的值也是1而不是2. 不过，这个bug在后续的版本中会修复的。
Opera Mobile 10不支持，不过Opera Mobile 12正确支持。
UC总是显示1，不过其viewport属性有些让人费解。
只有最近的Chrome浏览器才能正确实现该属性。Chrome19返回不准确的1, Chrome22可以正确返回2.
MeeGo WebKit (Nokia N9/N950)就吓人了：当你应用了meta viewport时候（类似<meta name="viewport" content="width=device-width">），值会从1变成1.5!

在css中，可以通过
-webkit-device-pixel-ratio，
-webkit-min-device-pixel-ratio和 
-webkit-max-device-pixel-ratio
进行媒体查询，对不同dpr的设备，做一些样式适配(这里只针对webkit内核的浏览器和webview)。
```
> 多屏适配

公式： `rem = document.documentElement.clientWidth * dpr / 10`

font-size示例：http://div.io/topic/1092

```
// Javascript 方式
iphone3gs: 320px / 10 = 32px
iphone4/5: 320px * 2 / 10 = 64px
iphone6: 375px * 2 / 10 = 75px

// css 方式
html{font-size: 32px;}
//iphone 6 
@media (min-device-width : 375px) {
   html{font-size: 64px;}
}
// iphone6 plus 
@media (min-device-width : 414px) {
    html{font-size: 75px;}
}
*/
```

### 移动端兼容问题

```
1、tap事件穿透（解决方案：click事件代替， 或者尝试fastclick这个框架）
2、fixed的元素有input框时在ios上的bug（可以考虑头和底部定高，中间加上一个iScroll的内容区域实现头尾固定，中间内容滑动的UI交互布局）;
3、离线缓存更新成功后必须刷新页面才能显示新的修改（写个全局的方法，监听updateready后，主动帮用户刷新一次页面）;
4、UC浏览器不支持alert（建议用自己通用的弹窗方法）;
5、同样的zepto写的选择器，有时候层级过深在某些浏览器中失效（在节点class和id上命名上合理分配，用常规选择器串）;
6、QQ浏览器SVG失效;
7、chrome和小米自带的手机浏览器，开发调试时不走代理（可以下载chrome的beta版）

========================================================

1、最痛恨的是红米手机，ua返回iphone，需要结合platform判断，但是还不准确，导致需要ios和android区别对待的时候就坑了。
2、是fixed的问题。这个解决办法是尽量不要用，不过ios7及以下才会出现这个问题。某些情况下红米也会有这个问题。（最近刚刚遇到，已经被坑挂了）。
3、如果你想要使用css3的动画，那么一定要变着方式使用3d gpu加速的方式，不要试着left，height，width这样的元素进行变换了，android4.4以下版本卡死你。
4、ios全线点击会有300毫秒延迟，使用fastclick解决。这个插件最良心了。
5、web app像素眼设计会纠结你1px边框问题。解决办法有相应知乎大牛答过。
6、qq浏览，uc浏览以及ios的浏览器，滚动时不会触发scroll事件，但会触发touchmove。当停止滚动后会出发scroll。
7、滚动有iscroll插件，但是还是使用原生的比较好。
8、meta功能要用好，禁止缩放，缩放比例，屏蔽电话号码等功能很实用。（手机回答就不列举了）。
9、如果想要像手机淘宝那样的各个平台看起来展示效果一致，那么就使用rem来做单位。
10、-webkit-tap-highlight-color可以取消点击高亮。
11、localStorage在浏览器开启无痕模式下ios会抛异常，导致js中断。
12、一些情况下对非可点击元素监听click事件，ios下不会触发，css增加cursor:pointer就搞定了。当然想要干脆静止点击就是not-allowed。
13、android4.4以下版本，设置圆角属性需要在直接元素上，向父元素设置圆角并且指定overflow:hidden是不会生效的。

1、运营商劫持文件命名（CSS/JS/HTML）如game、ad等


scroll元素临界的bug 
android screen.w/h 不准 
rem不准 
scroll时动画失效 
animate回调 
最小字号限制 
不同机型全屏自适应 android，ios和native通讯 下载app方案ios，android，微信都不一样 二维码识别

border-radius不要随便乱用，在很多安卓机型上都会出现锯齿，非常丑

IOS8 的 webview 加载网页，不支持使用 createElement 创建 video 元素播放 hls 视频流，然后动态指定 parent，会导致 app 崩溃。并且，webview 视频内嵌播放模式下点击全屏按钮会导致 app 崩溃。



1.iPhone5以上各种应用的webview（例如微信）在遇到有大量图片的页面时会出现图片乱套的情况，6和6plus更为严重，具体表现为各种img和background-image互相错乱，困扰了我们好久，简直是大坑，目前研究出暂时的解决方法是动态给所有用到图片的元素加上-webkit-transform : translate3d(0,0,0)
进行强制重绘，测试结果对于绝大部分出现问题的机子有效，但仍然有小部分机器还是会出问题，另一种方法是进行懒加载，但是这会降低开发效率，并且对使用background-image的元素比较难实现

2.safari（包括OS X、iOS和Windows版）对transform-origin的Z轴判定和其它所有浏览器都不同，设置transform-origin的Z轴会直接产生translateZ的变换，十分不理解，暂无纯css解法

3.Android调用重力传感器返回的数据和iOS和Windows Phone上的是相反的……

4.Android微信内置webview对meta viewport的支持有缺陷，当user-scalable=no时，设定width为固定数值（例如640）不会自动缩放，需要用js做一些处理（新版微信已经修正了这个问题）

5.Android的各种浏览器都不支持同时播放多个音频，并且通过js连续播放几个音频的时候会出现一些问题



0. Android 4.x 一些版本 input file 被从底层阉割，文件选择框 打不开也不报错 —— http://www.zhihu.com/question/21452742 

1. iScroll 5 官方示例 拖到本地开发环境就完全动不了，无论 Chrome 移动设备模拟器 还是 微信内置浏览器 —— 弃之不用，手写下拉刷新…… 

2. ECharts 的雷达图在 iOS WebView 中拖拽 容易使整个 ViewPort 高宽突破 meta 标签的限制 

3. Animate.css 中动作幅度较大的动画在用 腾讯 X5 内核的 App 中卡顿明显（鹅厂号称的基于 Chrome 完整内核优化 哪儿去了？MIUI 原生浏览器 都超流畅） 

4. WebKit 533-（微信、UC v9.8 开发者版）在遍历 带伪元素规则的 CSSStyleRule 对象时，会直接让浏览器崩溃（下断点走单步时基本没事，一释放断点立马崩溃，所以怀疑是栈溢出）—— 检测到老内核 直接跳过那段代码……（对老浏览器砍功能） 

5. a 标签 href 仅指向一个不存在的 hash，点击回调中根据 hash 内容调用相应功能，并阻止默认行为 —— Android、iOS 上各种 WebKit 浏览器 都会清空历史记录（执行了 a 标签的默认行为），导致单页应用无法回退……（桌面端浏览器 均无此问题） 

6. 微信 iOS 版 整页切换到 动态生成的表单，其提交按钮会自动点击，导致空白表单提交 —— 自己给 iOS WebKit 补上砍掉的 HTML 5 表单元素验证方法，提交前先验证 HTML 代码中的 required、pattern 属性，否则阻止提交事件


做过html5游戏的人应该深有体会webaudio有多坑android很多设备不支持，ios6和ios8的api不一样。
我现在的解决办法就是判断ios用webaudio，如果是android就用audio标签，但是还是无法完美解决。
```

#JAVASCRIPT

## 数据类型

> 类型转换

parseInt: 字符串转换成整数，同时还有基模式，即方法的第二个参数，如果十进制数包含前导0，那么最好采用基数10，这样才不会意外地得到八进制的值，如parseInt(010)＝8进制数

```
parseInt("1234blue");   //returns   1234 
parseInt("0xA");   //returns   10 
parseInt("22.5");   //returns   22 
parseInt("blue");   //returns   NaN
parseInt("AF",   16);   //returns   175  16进制
```

parseFloat：浮点型转换

```
parseFloat("1234blue");   //returns   1234.0 
parseFloat("0xA");   //returns   NaN 
parseFloat("22.5");   //returns   22.5 
parseFloat("22.34.5");   //returns   22.34 
parseFloat("0908");   //returns   908 
parseFloat("blue");   //returns   NaN
```

强制类型转换

- Boolean(value)——把给定的值转换成Boolean型； 
- Number(value)——把给定的值转换成数字（可以是整数或浮点数）； 
- String(value)——把给定的值转换成字符串。 
- Object.prototype.valueOf() 返回对象的原始值

```
var x = {
    toString: function () { return "foo"; },
    valueOf: function () { return 42; }
};

alert(x); // foo
"x=" + x; // "x=42"
x + "=x"; // "42=x"
x + "1"; // 421
x + 1; // 43
["x=", x].join(""); // "x=foo"
```

- instanceof 仅限类型比较

```
1 instanceof Number; //false
new Number(1) instanceof Number; //true
```

## 表达式&&运算符

> 运算符优先级与结合性：从左到右，从右到左
> 
http://www.cnblogs.com/dolphinX/p/3524977.html

- typeof 优先级最高 
- 赋值运算符的优先级最低

```
!、-(单目减)、++、--、typeof, new, void, delete 2
*、/、% 3
+、- 4
<<、>>、>>> 5
<、<=、<、>= 6
==、!=、===、!==、 7
& 8
^ 9
| 10
&& 11
|| 12
?: 13
=、+=、-=、*=、/=、%=、<<=、>>=、>>>=、&=、^=、|= 14
```

由右往左计算的运算符：

++、--、-、+、～、！、delete、typeof、void、?:、=赋值 *=、/=、+=、-=

由左往右计算的运算符：

*、/、%、+、-、+、<、<=、>、>=、instanceof、in、==、！=、&&、 ||

## OOP及其继承

1、原型链继承

```
  fuction parent(){}
  var child = new parent();
  
  child.prototype = new parent()
```

2、构造函数继承
```
function parent(){
   this.x = 6;
}
parent.prototype.send = function(){ console.log('s') }
function child(){
   parent.call(this);
   console.log(this.send)
}
(){ console.log('s') }
```
3、ES5 Object.create(xx.protype)
4、ES6 super


## 作用域
> 同名变量的查找顺序，后定义覆盖先定义。原型链是用来查找对象属性的。作用域链是一个对象列表或对象链

基础：https://github.com/kuitos/kuitos.github.io/issues/18

### setTimeout/setInterval

- 描述说明：http://www.cnblogs.com/hutaoer/p/3423782.html
- 执行原理：http://www.suchso.com/projecteactual/Javascript-setTimeout-timer.html

- 默认setTimeout(str)是全局作用域(window/global)
- settimeout延时为0的目标，是为了达到异步任务的作用，而异步会写到队列里所有会执行完同步任务才会去执行队伍里的异步任务

```
alert(1); 
setTimeout("alert(2)", 0); 
alert(3); 
虽然延时了0ms,但是执行顺序为：1，3，2 
```

一、setTimeout中的延迟执行代码中的this永远都指向window

二、setTimeout(this.method, time)这种形式中的this，即上文中提到的第一个this，是根据上下文来判断的，默认为全局作用域，但不一定总是处于全局下，具体问题具体分析。

三、setTimeout(匿名函数, time)这种形式下，匿名函数中的变量也需要根据上下文来判断，具体问题具体分析。在这里匿名函数的使用形成了一个闭包，从而能访问到外层函数的局部变量。这样子去理解，我觉得挺好的！只是这种闭包，跟常见的闭包不同，因为函数式放在setTimeout里面

## Function
> Function 对象的 length 属性，可以用来统计function的参数个数，例如func(x,y).length = 2

## 事件

> 冒泡和捕获

- 冒泡从特定事件目标冒泡到document
- 捕获从最不精确的（document）开始捕获，也可以从窗口级别捕获。
- 捕获先于冒泡
- 捕获事件的陷阱和对策：http://hax.iteye.com/blog/162718 不同的浏览器对event Flow定义不一样，有可能导致事件不被捕获或者调用两次的可能。

## Ajax
- 创建对象: 

```
 new XMLHttpRequest():  Chrome, IE7+, Firefox, Safari, and Opera
 
 new ActiveXObject("Microsoft.XMLHTTP"): IE6 
```

- Request

```
// GET PUT POST PATCH DELETE
xhttp.open("GET", "ajax_info.txt", true); // 第三个参数 false代表同步
xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); // 设置头
xhttp.send("fname=Henry&lname=Ford"); // 如果是POST请求,则传入string
```
- Response

```
xhr.responseText
xhr.responseXML
xhr.onreadystatechange 
xhr.readyState
xhr.status
xhr.responseType 
```

```
var xhttp, xmlDoc, txt, x, i;
  xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
  if (xhttp.readyState == 4 && xhttp.status == 200) {
    xmlDoc = xhttp.responseXML;
    txt = "";
    x = xmlDoc.getElementsByTagName("ARTIST");
    for (i = 0; i < x.length; i++) {
      txt = txt + x[i].childNodes[0].nodeValue + "<br>";
    }
    document.getElementById("demo").innerHTML = txt;
    }
  };
  xhttp.open("GET", "cd_catalog.xml", true);
  xhttp.send();
```
XMLHttpRequest: https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest
XMLHttpRequest2: http://www.html5rocks.com/zh/tutorials/file/xhr2/

更多：

```
 - xhr.overrideMimeType
    //overrideMimeType() 必须在 send() 之前设置
    xhr.overrideMimeType("text/plain; charset=utf-8");  
 
 - xhr.abort 中止
 
 
```
## 跨域

跨域：https://segmentfault.com/a/1190000000702539

跨域2：https://segmentfault.com/a/1190000000702550

总结：http://www.cnblogs.com/rainman/archive/2011/02/20/1959325.html

- iframe

- jsonp

- cors: Access-Control-Allow-Origin

- domain

- postmessage html5

## DOM操作

- createElement // 创建元素
- querySelector/querySelectorAll // 查询
- node.parentElement/node.parentNode 
- node.children
- node.getElementsByTagName/getElementsByClassName 查询子元素
- el. firstElementChild/lastElementChild
- el. nextElementSibling/previousElementSibling
- el. appendChild/removeChild/replaceChild
- parentElement.insertBefore(newElement, referenceElement)
- el.attributes /// 获取一个{name, value}的数组
- el. getAttribute/setAttribute
- el. hasAttribute/removeAttribute
- el. hasAttributes // 是否有属性设置
- doc. createDocumentFragment
- innerText/innerHTML 
- event.target

## 数组方法

- concat
- join
- pop
- push
- reverse
- shift 删除并返回数组的第一个元素
- slice
- sort
- splice  删除元素，并向数组添加新元素。splice(index,howmany,item1,.....,itemX)
- unshift 数组开头添加一个或多个元素

## ES5方法

更多： https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object

### Object

- Object.create(proto[, propertiesObject])
- `__proto__` 指向其构造器的prototype，即父类的prototype
- Object.defineProperties(obj, props)

```
Object.defineProperties(obj, {
  "property1": {
    value: true,
    writable: true
  },
  "property2": {
    value: "Hello",
    writable: false
  }
  // etc. etc.
});
```

- Object.defineProperty(obj, prop, descriptor)
- Object.getOwnPropertyNames
- Object.getOwnPropertyDescriptor

## 排序
> 冒泡排序，去重

- 冒泡排序:

1: https://segmentfault.com/a/1190000000471260
2: http://www.cnblogs.com/georgewing/archive/2008/12/17/1356916.html

- 快速排序：http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html

- 更多排序：http://developer.51cto.com/art/201102/244855.htm
- 去重: http://www.benben.cc/blog/?p=335
- 去重2：http://hao.jser.com/archive/4248/

## 模块化

> AMD/CMD/UMD: UMD = AMD + CMD


```
AMD | 速度快 | 会浪费资源 | 预先加载所有的依赖，直到使用的时候才执行
CMD | 只有真正需要才加载依赖 | 直到使用的时候才定义依赖

CMD推崇依赖就近，可以把依赖写进你的代码中的任意一行，是弱依赖的一种，开发效率为先，性能较差
AMD是依赖前置的，换句话说，在解析和执行当前模块之前，模块作者必须指明当前模块所依赖的模块，是强依赖的一种，预先加载，性能较好

目前推崇的是依赖ES6 MODULE/Webpack/Browserify的集成方案去解决，实现按需加载

```

## 正则

## Promise原理




