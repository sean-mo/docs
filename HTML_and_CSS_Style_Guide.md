# HTML and CSS Style Guide

## EditorConfig

建议选择支持 [EditorConfig](http://editorconfig.org/) 的编辑器，并安装相应插件，在项目根目录建立一个 `.editorconfig` 并插入以下内容：

```
[*.html]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = space
indent_size = 2
trim_trailing_whitespace = true

[*.ejs]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = space
indent_size = 2
trim_trailing_whitespace = true

[*.jade]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = space
indent_size = 2
trim_trailing_whitespace = true

[*.css]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = space
indent_size = 2
trim_trailing_whitespace = true

[*.less]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = space
indent_size = 2
trim_trailing_whitespace = true
```

## File Naming Conventions

- 使用半角小写英文字母（单词）和数字
- 使用半角的 `-` 和 `.` 分割单词
- 如非必要，请勿包含版本号

示例：

	my-cart.html
	style.css
	icon-sprites.png

## Indentations

每一级缩进使用两个空格，请勿混合使用空格和 tab。

	<nav>
	  <a href="//feifei.com">首页</a>
	  <a href="//feifei.com/my-account">我的账户</a>
	</nav>

	body {
	  color: #333;
	}

## Cases

使用小写编写 HTML 元素名、属性、除 `text/CDATA`、`data-attr` 外的属性的值和 CSS 选择器、属性、除字符串类型外的属性的值。

	<script src="//domain.com/js/lib/jquery.js"></script>
	<input type="email" required placeholder="请输入 Email 地址">

	body {
	  background-color: #efefef;
	}

	.cash {
	  &::before {
	    content: 'CNY';
	  }
	}

## File Encodings

确保编辑器 / IDE 使用的编码为 UTF-8 without BOM。如不能确定使用的编辑器 / IDE 的编码，请使用 Sublime Text、WebStorm 等工具，并保持默认编码设置。

HTML 需确保有以下语句：

	<meta charset="utf-8">

CSS 请勿加入以下语句：

	// bad
	@charset 'UTF-8';

## Protocols

引用 `http`、`https` 资源时尽量使用 `//`。

	// bad
	<script src="http://domain.com/js/lib/jquery.js"></script>

	body {
	  background-image: url(http://domain.com/img/main-bg.png);
	}

	// good
	<script src="//domain.com/js/lib/jquery.js"></script>

	body {
	  background-image: url(//domain.com/img/main-bg.png);
	}

## Tag Your Codes

使用 `#@tag` 在代码标记行为：

- `#@todo`：标记需尽快执行的事项
- `#@issue`：标记暂时可不执行的事项

HTML:

	<!-- #@todo: 购物车商品列表 -->
	<ul class='cart-item'></ul>

	<!-- #@issue: IE 8- 无法正确显示 section -->
	<section></section>

CSS:

	.add-cart-link {
	  /* #@todo: 添加 IE 8- 支持 */
	  opacity: .5;

		&:hover {
	    opacity: 1;
	  }
	}

	body {
	  /* #@issue: 只有 WebKit 支持 */
	  -webkit-font-smoothing: antialiased;
	}

使用示例：

命令行 + [ag](https://github.com/ggreer/the_silver_searcher) = `ag -i -A 2 -B 2 '#@(todo|issue)'`：

	HTML_and_CSS_Style_Guide.md
	111-使用 `#@tag` 在代码标记行为：
	112-
	113:- `#@todo`：标记需尽快执行的事项
	114:- `#@issue`：标记暂时可不执行的事项
	115-
	116-HTML:
	117-
	118:	<!-- #@todo: 购物车商品列表 -->
	119-	<ul class='cart-item'></ul>
	120-
	121:	<!-- #@issue: IE 8- 无法正确显示 section -->
	122-	<section></section>
	123-
	125-
	126-	.add-cart-link {
	127:	  /* #@todo: 添加 IE 8- 支持 */
	128-	  opacity: .5;
	129-
	134-
	135-	body {
	136:	  /* #@issue: 只有 WebKit 支持 */
	137-	  -webkit-font-smoothing: antialiased;
	138-	}

## HTML

### Short Doctype

使用 `<!DOCTYPE html>` 作为文档类型。注意大小写。

	// bad
	<!doctype html>
	<!DOCTYPE HTML>

	<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

	// good
	<!DOCTYPE html>

### X-UA-Compatible

在 `<head>` 加入 `X-UA-Compatible` 字段：

	<meta http-equiv="X-UA-Compatible" content="IE=Edge">

`X-UA-Compatible` 可以让 IE 一直跑在系统里已有的最高文档模式，除非用 DevTool 强制指定文档模式。

### Shell Browsers

在 `<head>` 加入 `name="renderer"` 字段，详见 [360 v6 浏览器内核控制 Meta 标签说明文档](http://se.360.cn/v6/help/meta.html)：

	<meta name="renderer" content="webkit">

### DNS Prefetch

使用 [DNS Prefetch](http://www.chromium.org/developers/design-documents/dns-prefetching) 来提高性能：

	<meta http-equiv="x-dns-prefetch-control" content="on">
	<link rel="dns-prefetch" href="//domain.com/">

### Optional Attributes and Tags

省略可省略的闭合标签、属性和属性值（可选）：

	<meta charset="utf-8">
	<link rel="stylesheet" href="style.css">
	<script src="jquery.js"></script>

	<input name="username">
	<input type="email" name="email" required>
	<button type="submit" disabled>Submit</button>

	<ul>
	  <li><p>Hell World
	</ul>

### Quotes

使用双引号包裹属性值：

	// bad
	<input type='text'>

	// good
	<input type="text">

### Blocks

每个块、列表、表格元素之间用空行分隔：

	<p>Nice to meet you!

	<ul>
	  <li>Hello
	  <li>Feifei
	</ul>

	<table>
	  <tr>
	    <td>Order ID
	  </tr>
	</table>

### Comments

- 注释和代码块之间以一行空行分隔

示例：

	<div class="placeholder-hack">
	  <input>
	  <!-- Only visible in IE 6-8 -->
	  <span class="placeholder"></span>
	</div>

	<!-- main header nav bar -->
	<nav>
	  <a href="/index">Home</a>
	  <a href="/my-account">My Account</a>
	</nav>
	<!-- end main header nav bar -->

### Indentations

严格按照层级关系缩进。每级缩进为两个空格。

	// bad
	<html>
	<head>
	<title></title>
	</head>
	<body>
	<div>
	  <p>
	</div>
	</body>
	</html>

	// good
	<html>
	  <head>
	    <title></title>
	  </head>
	  <body>
	    <div>
	      <p>
	    </div>
	  </body>
	</html>

### "alt" and "title" Attributes

尽量加上 `alt` 和 `title` 属性：

	// bad
	<a href="/index">Home</a>
	<img src="logo.png">

	// good
	<a href="/index" title="Home Page">Home</a>
	<img src="logo.png" alt="Site Logo">

### Entity References

在 UTF-8 编码下，可不用字符实体的，一律不用。

## LESS

### Naming Convensions

#### General Rules

- 使用半角小写英文字母和数字（不能以数字开头）
- 使用半角 `-` 分割单词（第三方库保留原命名，无需更改）
- 使用可读性好、描述抽象实体或行为而非样式的命名
- 可在 ID / class 前添加模块化前缀
- 在可以理解且通用的情况下，可缩短 ID 和 Class 的命名

示例：

	// bad
	.left
	.btn-green
	.button-large
	.ftr
	#navigation

	// good
	.pull-left
	#nav
	.btn
	.btn-large
	.logo
	.dropdown-menu-list

#### Variables

在变量后添加类型标识，如 `font-size`、`color`；可同时使用 `light`、`base` 等关键字进一步区分：

	@gray-light: #ccc;
	@sans-font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
	@base-font-size: 14px;
	@warning-text-color: @danger-light;
	@warning-background-color: @danger-light;
	@box-border-radius-base: 3px;

#### Mixins

Mixin 需以 `()` 结尾；在可通用的场合，可以使用 class 来代替。

	// bad
	.borderless {
	  border: 0 none;
	}

	// good
	.borderless() {
	  border: 0 none;
	}

	.clearfix {
	  // ...
	}

	.box {
	  .clearfix;
	}

### Quotes

使用单引号：

	// bad
	body {
	  font-family: "Avenir New";
	}

	// good
	body {
	  font-family: 'Avenir New';
	}

### Selectors

- 优先使用 class 选择器
- ID 选择器用于样式唯一的元素
- 多个选择器需分行写，一行一个，逗号置于行尾，左大括号置于最后一个选择器尾部，并以一个空格隔开
- 避免标签选择器和 ID 或 class 选择器混用
- 避免冗长的选择器，尽量保持在 3~4 个以内
- 避免使用 `*`

示例：

	// bad
	ul.online-users {}
	button.login {}
	#send-weibo.btn {}
	div.content {}
	.common-header .main-nav ui li {}

	[type=text], [type=email] {}

	// good
	.main-menu {}
	.online-users {}
	.btn-login {}
	.main-nav li {}

	[type=text],
	[type=email] {}

### Values and Units

- 值和单位使用小写；
- 使用简写的十六进制颜色值；
- 省略 `0` 后的单位；
- 如果值小于 `1`，省略小数点前的 `0`。

示例：

	/* bad */
	header {
	  color: #DCDCDC;
	  font-size: 12PX;
	  margin: 0px;
	  opacity: 0.7;
	}

	/* good */
	header {
	  color: #ddd;
	  font-size: 12px;
	  margin: 0;
	  opacity: .7;
	}

## Commas, Whitespacesm, Colons and Semicolons

- 逗号置于单词后面，并在逗号后面加入空格
- 冒号需紧跟着属性，和值之间以一个空格分割
- `([` 后不需要插入空格，`)]` 前不需要插入空格
- 请不要省略分号

示例：

	// bad
	[ type=text ],[type=email]{
	  font-family:Avenir,Arial;
	  font-weight: 400
	}

	.btn {
	  background-image: url( btn-bg.png );
	}

	// good
	[type=text],
	[type=email] {
	  font-family: Avenir, Arial;
	  font-weight: 400;
	}

	.btn {
	  background-image: url(btn-bg.png);
	}

### Properties

- 属性以字母顺序排列，前缀不参与排序，但前缀之间需排序，W3C 标准语法放在末尾
- 优先使用属性的简写，但注意避免不必要的复写
- LESS mixin 置于最上，以字母排序
- LESS mixin 若无参数，可省略 `()`
- LESS mixin 和标准 CSS 属性之间以一个空行分割

示例：

	// bad
	body {
	  font: 16px/1.5 Arial;
	}

	h1 {
	  font: 16px/1 Avenir, Arial;
	}

	nav {
	  margin-bottom: 10px;
	  margin-left: 10px;
	  margin-top: 10px;
	}

	div {
	  width: 100px;
	  margin: 10px;
	  box-sizing: border-box;
	  -webkit-box-sizing: border-box;
	  -moz-box-sizing: border-box;
	  .clearfix();
	}

	// good
	h1 {
	  font-family: Avenir, Arial;
	  line-height: 1;
	}

	nav {
	  margin: 10px 10px 10px 0;
	}

	nav li {
	  padding: 10px;
	}

	nav li li {
	  padding-left: 30px;
	}

	div {
	  .clearfix;

	  -moz-box-sizing: border-box;
	  -webkit-box-sizing: border-box;
	  box-sizing: border-box;
	  margin: 10px;
	  width: 10px;
	}

### DRY

- 将可复用部分抽象成基础的变量、class 或 mixin
- 请勿在没有 context 的情况下使用带强烈行为、样式含义的命名，如 `.red`、`.left`

示例：

	// bad
	.red {
	  background: red;
	}

	.two {
	  .column {
	    width: 100% / 2;
	  }
	}

	// right 是状态描述
	.right {
	  float: right;
	}

	// good
	@text-color-danger: red;

	.pull-right {
	  float: right;
	}

	.sidebar {
	  &.left {
	    margin-right: 20px;
	  }

	  &.right {
	    margin-left: 20px;
	    margin-right: 0;
	  }
	}

	.text {
	  &.red {
	    color: @text-color-danger;
	  }

	  &.text-red {
	    color: @text-color-danger;
	  }
	}

	.grid {
	  &.two {
	    .column {
	      width: 100% / 2;
	    }
	  }

	  &.three {
	    .column {
	      width: 100% / 3;
		  }
	  }
	}

### Browser Compatibilities

使用 [Modernizr](http://modernizr.com/) 和条件注释来区分不同样式。

	<html class="js no-touch cssanimations csstransforms csstransforms3d csstransitions video audio localstorage sessionstorage boxsizing mediaqueries placeholder no-ie8compat formvalidation">
	  <!--[if IE 6]>
	  <body class="ie ie6">
	  <![endif]-->
	  <!--[if IE 7]>
	  <body class="ie ie7">
	  <![endif]-->
	  <!--[if IE 8]>
	  <body class="ie ie8">
	  <![endif]-->
	  <!--[if not IE ]>
	  <body>
	  <![endif]-->

	.placeholder {
	  input {
	    &::-webkit-input-placeholder {}
	  }

	  .placeholder-polyfill {
	    display: none !important;
	  }
	}

	.no-placeholder {
	  input {}

	  .placeholder-polyfill {
	  	display: block;
	  }
	}

#### Hacks

一般而言，避免使用如下的 hacks：

- `property: \0`（IE 8 - 10）
- `property: value\9\0`（IE 9 - 10）
- `*property: value`（IE 6 - 7）
- `_property: value`（IE 6）

在 IE 6-7 下简短几句属性可解决的问题可以使用 hacks：

	* {
	  box-sizing: border-box;
	  *behavior: url(boxsizing.htc);
	}

	.clearfix {
	  *zoom: 1;
	}

### Comments

置于头部的文件描述：

	//
	// What is this file used for
	// --------------------------------------------------
	// Long description, optional

分组代码，示例：

	// Global values
	// --------------------------------------------------

	// Grays
	// -------------------------
	// these color values are fixed

	@black-10: darken(#fff, 10%); // #e6e6e6

	@gray-lighter: #eee;

	// Accent colors
	// -------------------------

	@red-light: #c00;

	// Component colors
	// -------------------------

	@success-background-color: @green-light;

在定义变量时，如值不能被直观地辨认，在其后使用注释标明实际值，如：

	@black-10: darken(#fff, 10%); // #e6e6e6

示例：

	// Imported libraries/frameworks
	// --------------------------------------------------
	// Imported libraries/frameworkds should on the top of custom styles

	// Normalize.css provides more robust code than reset.css
	@import 'base/normalize'

	// Import Fly UI as base framework
	@import 'fly-ui/fly-ui'

	// Custom Styles
	@import 'base/variables'
	@import 'base/mixins'

	// Page-specified variables
	// --------------------------------------------------

	// This only used in this page
	@_blue-lighter: #0084b7;

	// Page-specified rules
	// --------------------------------------------------

	body {
	  background-color: @gray-lighter;
	  color: @black-80;
	}

	// Reset input fields
	// -------------------------

	input[type="text"],
	input[type="password"],
	input[type="email"],
	textarea { /* … */ }


### Tools

LESS 会经过自动化工具编译，如果希望手工编译，可以使用以下工具：

- Linux: [Koala](http://koala-app.com/)
- OS X & Windows: [Prepros](http://alphapixels.com/prepros/), [LiveReload](http://livereload.com/)
- OS X: [CodeKit](http://incident57.com/codekit/), [LESS.app](http://incident57.com/less/), [SimpLESS](http://wearekiss.com/simpless)
- Windows: [SimpLESS](http://wearekiss.com/simpless), [Crunch](http://crunchapp.net/), [Smalify](http://smalify.net/)

## Meta

- 在每个文件的末尾添加一行空行
- 请删除行尾多余的空格