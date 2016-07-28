# Chrome Dev Tools

> 基本console

1、console.log 用于输出普通信息

2、console.info 用于输出提示性信息

3、console.error用于输出错误信息

4、console.warn用于输出警示信息

5、console.debug用于输出调试信息

6、console.dirxml输出某个node结点的信息

> 断言

假如childNodes数量小于500，则断言

```
console.assert(list.childNodes.length < 500, ‘Node Count is > 500’)
```
> 分组

- 显示分组，允许嵌套 

```
 console.group("Task Group"); // 可以用groupCollapsed进行拆叠
    console.log("Starting Sub Task A");
    console.group("Task Group A");
	    console.log("Task Stage 1 is completed");
	    console.log("Task Stage 2 is completed");
	    console.log("Task Stage 3 is completed");
	 console.groupEnd();
 console.groupEnd();
```
    
- 可以代替console.group，用于分组折叠

```
 console.group("Task Group"); // 可以用groupCollapsed进行拆叠
    console.log("Starting Sub Task A");
    console.groupCollapsed("Task Group A");
	    console.log("Task Stage 1 is completed");
	    console.log("Task Stage 2 is completed");
	    console.log("Task Stage 3 is completed");
	 console.groupEnd();

```
> 格式化

**console.log(“‘%s’”, xx)**

```
%s 字符串
%d 或 %i 整型
%f 浮点
%o 将值格式化为可张开的DOM对象 console.dir 也可以
%O 将值格式化可张开的JAVASCRIPT对象
%c 第二个参数传进去css样式
///传样式
console.log("%cThis will be formatted with large, blue text", "color: blue; font-size: x-large”);
```

> 测试耗时

```
console.time('key')
var a = 1;
console.timeEnd('key')
```

> 统计代码执行的次数

```
function myfun(){
	// 其它函数...
	console.count('myfun被执行的次数');
}
myfun();
```
> 打印对象或DOM

```
console.dir({
	a: '1',
	b: '2',
	c: true,
	myfunc: function(){
		console.log('xxx')
	}
})
```
> 设置条件断点

可以设置条件断点 http://www.randomthink.net/blog/2012/11/breakpoint-actions-in-javascript/

即在sources里设置断点后，编辑断点，输入表达式，如 abc = 2;

命令行api：https://developer.chrome.com/devtools/docs/commandline-api


> $: 支持Jquery的$

getEventListeners(object): 获取对象的事件

> monitor(function) 监视函数的输入参数和返回值

function add(x,y) => x + y;
monitor(add);
unmonitor(add)

> 监控对象事件 

- monitorEvents(object[, events]) 
- monitorEvents(windows, [‘resize’, ’scroll’])
- unmonitorEvent(object)

事件        支持事件
mouse	"click", "dblclick", "mousedown", "mouseeenter", "mouseleave", "mousemove", "mouseout", "mouseover", "mouseup", "mouseleave", "mousewheel"
key	"keydown", "keyup", "keypress", "textInput"
touch	"touchstart", "touchmove", "touchend", "touchcancel"
control	"resize", "scroll", "zoom", "focus", "blur", "select", "change", "submit", “reset"

> 获取当前CPU消耗的propfile

- propfile(‘x’)
- propfileEnd(‘x’)

> 表格化显示

```
table(data[, columns])  data可以是个JSON对象
var names = [ {x:1,y:2}, {x:3, y:6}]
table(names)
```
> 开发流程

```
cmd + o 文件查找
cmd + f 文本查找
cmd + shift + o 函数查找
cmd + g  跳至行号
```

> “开发者工具”也会保存本地文件的所有修改记录。如果你编辑了一个脚本或者样式并进行了保存，那么可以在Sources面板里的文件名上单击右键（或者在源代码区域），然后选择“本地修改(Local modifications)”来查看历史记录。
出现的本地修改(Local modifications)面板将会显示：

修改内容的差异；
修改的时间；
修改的文件名和一些操作链接。

“还原(revert)”链接将会还原所有对文件的修改至初始状态，并删除历史记录。
“恢复原始内容(apply original content)”将会完成相同的动作，但是会在视图里保留历史记录，以便于还原某个特定的修改。
最后，“应用修改的内容(apply revision content)”将会使在某个时间发生的修改生效。

可以右键设置DOM断点break on 当该dom发生变化的时候自动断点
subtree modification 当子树被修改
attribute … 当属性被修改
node remove 当结点被删除

## 使用 Chrome 远程调试 Android 设备
https://github.com/sean-mo/CN-Chrome-DevTools/blob/master/md/Use-Tools/remote-debugging.md

