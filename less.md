
## 编码规范

### BEM 规范
> BEM代表块（Block），元素（Element），修饰符（Modifier）。
> [进入相关介绍](http://www.w3cplus.com/css/bem-definitions.html)

- 块：块代表一个组件，可以简单的或者复合的
- 元素：是块的一部分，代表一个组件的功能，依赖于块中才能够有意义
- 修饰符：代表对块进行修饰的状态样式，例如hover,current,selected等

**注意：BEM最多只能有B+E+M三级，不能出现 B+E+E+..+E+M 超长class名**

#### 常见的BEM规则

```
.block {}
.block__element {}
.block--modifier {}
.block-name--element-name {}
.blockName-elementName {}
.blockName-elementName--modifierName {}
.block-name--modifier-name {}
.block-name__element-name {}
.block-name__element-name--modifier-name {}
```

#### LESS的BEM写法
> 例如是菜单项

```
//  html
<div class="menu menu--horizontal">
  <div class="menu__item">Home</div>
  <div class="menu__item">About</div>
  <div class="menu__item">Contact</div>
</div>

// css
.menu {
  display: block;
}

.menu__item {
  display: inline-block;
  line-height: 30px;
  width: 100px;
}

.menu--horizontal {
  width: 100%;
  height: 30px;
}

.menu--vertical {
  width: 100px;
  height: 100%;
}

.menu--horizontal .menu__item {
  border-right: 1px solid #e5e5e5;
  text-align: center;
}

.menu--vertical .menu__item {
  border-bottom: 1px solid #e5e5e5;
  text-align: left;
}

// less
.menu {
  display: block;

  &__item {
    display: inline-block;
    line-height: 30px;
    width: 100px;
  }

  &--horizontal {
    width: 100%;
    height: 30px;
  }

  &--vertical {
    width: 100px;
    height: 100%;
  }
}
```

## 组件规范

### 按钮规范
> 根据常见的按钮尺寸、类型、状态等规范样式，规则： button-element

- 尺寸：
    - 大号按钮（btn-large)
    - 中号默认按钮（btn)
    - 小号按钮(btn-small)
- 类型
	- 默认（btn)
	- 首选（btn-primary)
	- 成功（btn-success）
	- 一般信息（btn-info)
	- 警告（btn-warning)
	- 危险（btn-danger）
	- 链接（btn-link)
   - 虚线按钮（btn-dashed）
   - 幽灵按钮（btn-ghost）
   - 加载按钮（btn-loading）
- 组合按钮
   btn-group
- 状态
    - 图标按钮（btn-icon-only)
    - 图文混合按钮（btn-icon): btn-icon-left（左） btn-icon-right（右）
    - 激活（btn-active）
    - 禁用（btn-disabled）

### 图片规范
> 根据常见的图片布局方式、形状等规范样式，规则： img--modifier
 
- 布局方式
  - 响应式：(img-responsive) 

- 形状
  - 圆角: (img-rounded)
  - 圆形：(img-circle)
  - 缩略图：(img-thumbnail)
  
### 导航条规范
> 根据常见的导航条形状、颜色等规范样式，规则： nav--modifier

- 形状
  - 默认（nav）
  - 胶囊式 （nav--pills）
  - 垂直胶囊式 （nav--stacked）
  - 两端对齐 （nav--justified）
  - 下拉菜单 （nav--tabs）
  
- 颜色
  - 反色（nav--inverse）
  
###  分页规范
> 根据常见的分页尺寸、功能等规范样式，规则： pagination--modifier

- 尺寸
    - 默认（pagination）
    - 大（pagination--lg）
    - 小（pagination--sm）
- 功能
    - 当前（pagination--active）
    - 不可用（pagination--disabled）

### 进度条规范
> 根据常见的进度条状态、样式等规范样式，规则： progress--modifier

- 情景
    - 默认（progress-bar）
    - 成功（progress-bar-success）
    - 一般信息（progress-bar-info）
    - 警告（progress-bar-warning）
    - 危险（progress-bar-danger）
- 样式
    - 条纹（progress-bar-striped）
    - 动画（progress-bar-striped_active）
    
### 表格
> 根据常见的表格样式，状态等规范样式，规则：table--modifier

- 样式
    - 条纹（table--striped）
    - 边框（table--bordered）
    - 紧缩（table--condensed）
- 状态
    - 鼠标悬停当前（table--active）
    - 标识成功或积极的动作（table--success）
    - 标识普通的提醒信息的动作（table--info）
    - 标识警告或者需要用户注意的动作（table--warning）
    - 标识危险或者潜在负面影响的动作（table--danger）
    
###  级联选择
> 根据常见的级联选择样式，状态等规范样式，规则：cascader--modifier

- 样式
    - 默认（cascader--input）
    - 下拉列表（cascader--menu）
       - 下拉项（cascader-menu-item）
- 状态
    - 禁用（cascader-menu-item-disabled）

### Checkbox多选框
> 根据常见的Checkbox样式，状态等规范样式，规则：checkbox--modifier 
   
- 样式
    - 默认（checkbox--input）
    - Checkbox列表（checkbox-group-item）
- 状态
    - 禁用（checkbox--disabled）
    - 激活（checkbox--checked）
    
### 日期选择框
> 根据常见的celendar样式，状态等规范样式，规则：celendar--modifier 

- 样式
    - 默认（calendar-range-picker）
    - 大 （input--lg）
    - 小 （input--sm）
- 状态
    - 禁用（calendar--disabled）
    
### Input 输入框
> 根据常见的input样式规范样式，规则：input--modifier 

- 样式
    - 大（input--lg）
    - 默认（input）
    - 小（input--sm）
    
### Radio 单选框
> 根据常见的Radio样式，状态等规范样式，规则：radio--modifier 

- 样式
    - 默认（radio--input）
    - Radio组（radio--group）
- 状态
    - 禁用（radio--disabled）
    - 激活（radio--checked）

## 项目结构

### 基本结构

- config：变量（字体、字体大小、色值等）
- fonts：字体文件
- iconfont：淘宝图标字体
- fontAwesome：Bootstrap图标字体
- lib：外部或第三方样式
- mixins：通用LESS函数库（区分PC、移动端样式）
- modules：模块/页面
- component：组件

### 通用组件

- buttons: 按钮
- form: 表单
- tables：表格
- tabs： 选项卡
- caret：三角
- close： 关闭按钮
- switch: switch 开关
- badge：徽章
- progress：进度条
- modal：弹层 
- checkbox：复选框
- dropbox：下拉框
- calendar：日历
- tips：提示（包含正确、错误提示、确认提示等）
- ...

### 模块组件

- member-selector: 部门选择器
- filter-groups: 筛选组
- tip：提示模块：确认提示、选择提示
- talbe：基础表格模块：支持排序、筛选、自定义表头、设置默认展示等
- label：标签模块：包括筛选、设置标签
- switch：视图切换模块
- time：时间筛选项：更新时间
- pagination：分页模块
- ...

### 页面组件

- page-report: 日报
- page-index: 首页
- ...

### 通用函数（mixins）

- size: 设置元素的width/height/line-height的样式
- reset-filter: 重置（取消）滤镜
- Absolute-Center：IE8+ 元素重直居中（绝对定位）
- center-block： 水平居中
- clearfix：清除浮动
- radius: 圆角
- rotate|scale: 变形
- animate：动画
- text-overflow：截断文本
- box-shadow：阴影
- text-shadow：文字阴影
- border：边框 
- ...

## LESS 特性
> http://lesscss.cn/features/

### 变量（Variables）

```
// Variables
@link-color:        #428bca; // sea blue
@link-color-hover:  darken(@link-color, 10%);

// 选择器
@my-selector: banner;

.@{my-selector} {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}

// URLs
@images: "../img";

body {
  color: #444;
  background: url("@{images}/white-sand.png");
}

// Import
@themes: "../../src/themes";
@import "@{themes}/tidal-wave.less";

// 属性
@property: color;

.widget {
  @{property}: #0ee;
  background-@{property}: #999;
}
```

### 继承（Extend）

```
nav ul {
  &:extend(.inline);
  background: blue;
}


.big-division,
.big-bag:extend(.bag),
.big-bucket:extend(.bucket) {
  // body
}
```

### 混合
#### 无参
```
.border {
    border: 1px soid pink;
}

.box {
    .border;
}
```
==>
```css
.box {
  border: 1px soid pink;
}
```

#### 带参数
```css
.border_02(@border_width) {
    border: @border_width solid yellow;
}
.border_03(@border_width:30px) {
    border: @border_width solid yellow;
}

.boxer {
    .border_02(20px);
}
.boxer_02 {
   .border_03(); 
}
```
==>
```
.boxer {
  border: 20px solid yellow;
}
.boxer_02 {
  border: 30px solid yellow;
}
```

### 匹配
```css
 .triangle(bottom,@w: 5px,@c: #ccc){
 	border-width: @w;
 	border-color: @c transparent transparent transparent;
 	border-style: solid dashed dashed dashed;
 }
 .triangle(top,@w: 5px,@c: #ccc){
 	border-width: @w;
 	border-color:  transparent transparent @c transparent;
 	border-style:  dashed dashed solid dashed;
 }
 .triangle(left,@w: 5px,@c: #ccc){
 	border-width: @w;
 	border-color:  transparent @c transparent transparent;
 	border-style:  dashed solid dashed dashed;
 }
 .triangle(right,@w: 5px,@c: #ccc){
 	border-width: @w;
 	border-color:  transparent transparent transparent @c;
 	border-style:  dashed dashed dashed solid;
 }
 .sanjiao {
 	.triangle(right,10px,#000);
 }

```
==>
```css
.sanjiao {
  border-width: 10px;
  border-color: transparent transparent transparent #000000;
  border-style: dashed dashed dashed solid;
  width: 0;
  height: 0;
  overflow: hidden;
}
```

### 运算
```css
@test-1: 300px;
.box-02{
	background-color: #eee - 10;
	width: @test-1 + 200;
	height: @test-1 + 100;
}
```
==>

```css
.box-02 {
  background-color: #e4e4e4;
  width: 500px;
  height: 400px;
}
```


## LESS 常用函数
> http://lesscss.cn/functions/

### 杂项函数

```
// image-size： 获取图片尺寸
Example: image-size("file.png");
Output: 10px 10px

// image-width / image-height 
Example: image-width("file.png");
Example: image-height("file.png");

// convert：转换单位
Example:
convert(9s, "ms")
convert(14cm, mm)
convert(8, mm) // incompatible unit types
Output:
9000ms
140mm
8
```

### 列表函数

```
// length: 获取数组长度
@list: "banana", "tomato", "potato", "peach";
n: length(@list); // 4

```

### 类型函数

```
// ispixel：判断是否px单位
Example: 
ispixel(#ff0);     // false
ispixel(blue);     // false
ispixel("string"); // false
ispixel(1234);     // false
ispixel(56px);     // true
ispixel(7.8%);     // false
ispixel(keyword);  // false
ispixel(url(...)); // false

// isem： 判断是否em单位
Example: 
isem(#ff0);     // false
isem(blue);     // false
isem("string"); // false
isem(1234);     // false
isem(56px);     // false
isem(7.8em);    // true
isem(keyword);  // false
isem(url(...)); // false

// ispercentage： 判断是否为比例值
Example:
ispercentage(#ff0);     // false
ispercentage(blue);     // false
ispercentage("string"); // false
ispercentage(1234);     // false
ispercentage(56px);     // false
ispercentage(7.8%);     // true
ispercentage(keyword);  // false
ispercentage(url(...)); // false

```

### 颜色定义函数

```
// rgb :转化十六进制颜色值
Example: rgb(90, 129, 32)
Output: #5a8120
```

### 颜色操作函数
```
//mix: 混合颜色
Example:mix(#ff0000, #0000ff, 50%)
Output： #800080

```

## 书写规范
 - 颜色
    - 建议用小写十六进制，可以缩写，不用‘red’来表示颜色
 - 字体
    - 建议用单引号表示字体
    Example：a { font-family: 'Times New Roman', 'Times' }
 - 文字粗细
    - 建议用合适的数字或者名字书写 
     Example：
     a { font-weight: bold;}
     a { font-weight: 200;}
 - 数字
     - 建议去掉小数点前面的"0"
     Example：a {line-height: .5px;}
     - 建议小数点后面最多三位数
     - 建议值为0的单位默认缺省
     Example： a {line-height： 0;}
 - 字符串
     -  建议默认属性值引号用双引号（“”）
     Example： a { content: "x"; }   
 - 时间
    - 建议动画和过度时期少于100ms
 - 单位
    - 建议单位书写格式为小写
 - 缩写
    - 建议尽量缩写你的CSS
    Example：margin，padding，border-color，border-radius，border-style，border-width... 
 - 声明
    - 建议尽量少用!important属性值
    - 建议属性后面直接跟着":",加一个空格然后加上属性值
    - 建议一行声明一个属性，并用";"表示结束
    - 不允许空属性的样式
    - 不允许属性重复声明
    - 不允许声明无效的属性
      Example：
    ``` css
    display: inline used with width, height, margin, margin-top, margin-bottom, and float.
    display: inline-block used with float.
    display: list-item used with vertical-align.
    display: block used with vertical-align.
    display: flex used with vertical-align.
    display: table used with vertical-align.
    display: table-* used with margin (and all variants) or float.
    display: table-* (except table-cell) used with vertical-align.
    position: static used with top, right, bottom, and left.
    position: absolute used with float or vertical-align.
    position: fixed used with float or vertical-align.
    float: left and float: right used with vertical-align. 
    ``` 
     
## 参考链接

- [A New Front-End Methodology: BEM](https://www.smashingmagazine.com/2012/04/a-new-front-end-methodology-bem/)
- [html-inspector](https://github.com/philipwalton/html-inspector/blob/master/src/rules/convention/bem-conventions.js#L1-L27)
- [suitcss](http://suitcss.github.io/)
- [A BEM syntax with UX in mind](http://montagestudio.com/blog/2013/10/24/BEM-syntax-with-ux-in-mind/)
- [BEM定义](http://www.w3cplus.com/css/bem-definitions.html)
- [ANT DESIGN](http://ant.design/#/components/button)