# Typescript 语言规范

## 命名规范
- 命名需选择有意义的名字，除了默认成俗的名字外，尽量不要使用简写形式
- 虽然 JavaScript 支持 Unicode 字符，但请使用 [a-zA-Z0-9_\$]
- 使用驼峰命名法（camel case），变量和函数使用小驼峰，如 getElementById，类、构造函数和枚举类型使用大驼峰，如 `DocumentFragment`
- 常量所有字母均大写并使用 _ 分割单词，如 Number.MAX_VALUE
- 除了常量和枚举值以外，不要使用下划线’_’
- 尊重惯例，避免使用常用库、框架名，如 $ = {}、jQuery = {}、_ = {}
- Boolean 变量、函数请加上 is、has 前缀
- 当保存 this 对象时，尽量使用可对应上下文的命名，如report，或使用 that
- 不要使用过长的变量名（例如50个字符）。过长的变量名会导致代码丑陋（ugly）和难以阅读（hard-to-read），还可能因为字符限制在某些编译器上存在兼容性问题。
- 方法使用首字母小写的驼峰命名法：getStudentSchoolType
- 方法参数使用首字母小写的驼峰命名法：setSchoolName(schoolName)
- 使用有意义的方法参数命名，这样做可以在没有文档的情况下尽量做到“自解释（documentate itself）”

示例：

    // bad
    valid = true;
    k = 'word'
    hasAttr = function (e, a) {
      var _a = e.getAttribute(a);
      return _a === null;
    };

    function user (opts) {
      this.name = opts.name;
      this.ID = opts.ID;
    }

    $form.on('submit', function(){
      var that = this;
    });

    // good
    isValid = true;
    keyword = 'word';
    hasAttr = function (elem, attr) {
      var _attr = elem.getAttribute(attr);
      return _attr === null;
    }

    function User (options) {
      this.name = options.name;
      this.id = options.id;
    }

    $form.on('submit', function(){
      var rootForm = this;
      // or
      var _this = this;
    });
    
## 书写约定
> 配置TS-Lint，通过IDE自动化约定配置规范

### Typescript

- member-access: 显示的声明Class的成员修饰。即public/private需要指定
- member-ordering: 声明Class成员需要按照以下规则：
	- public 在 private 之前
	- static 在 instance 之前
	- variables 在 functions 之前
- no-var-requires: 禁止使用 `var module = require("module")`
- typedef: 类型必须声明
	- 函数必须有返回类型
	- 非箭头函数必须有类型
	- 箭头函数必须有类型
	- 接口的返回类型属性
	- 变量声明
	- 成员变量声明
- typedef-whitespace: 类型之间必须留有空白

### 其它更多约定

> 以下是完整的tslint规范，具体对应的规则，直接浏览[TsLint Rule](http://palantir.github.io/tslint/rules/)

```
{
  "rules": {
    "align": [
      true,
      "parameters",
      "statements"
    ],
    "ban": false,
    "class-name": false,
    "comment-format": [
      true,
      "check-space",
      "check-uppercase"
    ],
    "curly": true,
    "eofline": true,
    "forin": true,
    "indent": [
      true,
      "tab"
    ],
    "interface-name": false,
    "jsdoc-format": true,
    "label-position": true,
    "label-undefined": true,
    "max-line-length": [
      true,
      140
    ],
    "member-access": false,
    "member-ordering": [
      true,
      "public-before-private",
      "static-before-instance",
      "variables-before-functions"
    ],
    "no-any": false,
    "no-arg": true,
    "no-bitwise": true,
    "no-conditional-assignment": false,
    "no-consecutive-blank-lines": false,
    "no-console": [
      true,
      "debug",
      "info",
      "time",
      "timeEnd",
      "trace"
    ],
    "no-construct": true,
    "no-constructor-vars": true,
    "no-debugger": true,
    "no-duplicate-key": true,
    "no-duplicate-variable": true,
    "no-empty": true,
    "no-eval": true,
    "no-inferrable-types": false,
    "no-internal-module": false,
    "no-null-keyword": true,
    "no-require-imports": false,
    "no-shadowed-variable": true,
    "no-string-literal": true,
    "no-switch-case-fall-through": true,
    "no-trailing-whitespace": true,
    "no-unreachable": true,
    "no-unused-expression": true,
    "no-unused-variable": true,
    "no-use-before-declare": true,
    "no-var-keyword": true,
    "no-var-requires": true,
    "object-literal-sort-keys": false,
    "one-line": [
      true,
      "check-open-brace",
      "check-catch",
      "check-else",
      "check-whitespace"
    ],
    "quotemark": [
      false,
      "single",
      "avoid-escape"
    ],
    "radix": true,
    "semicolon": false,
    "switch-default": true,
    "trailing-comma": [
      false,
      {
        "singleline": "never",
        "multiline": "always"
      }
    ],
    "triple-equals": [
      true,
      "allow-null-check"
    ],
    "typedef": [
      true,
      "call-signature",
      "parameter",
      "property-declaration",
      "variable-declaration",
      "member-variable-declaration"
    ],
    "typedef-whitespace": [
      true,
      {
        "call-signature": "nospace",
        "index-signature": "nospace",
        "parameter": "nospace",
        "property-declaration": "nospace",
        "variable-declaration": "nospace"
      }
    ],
    "use-strict": [
      true,
      "check-module"
    ],
    "variable-name": [
      true,
      "check-format",
      "allow-leading-underscore",
      "ban-keywords"
    ],
    "whitespace": [
      true,
      "check-branch",
      "check-decl",
      "check-operator",
      "check-separator",
      "check-type"
    ]
  }
}
```


## 项目结构

- utils: 通用模块
- chunks: 按需加载模块
- components: 项目组件，包括基础组件（base）、业务模块（module）
- decorators: 修饰器
- redux: 单向数据流
- routes: 路由
- views: 容器/视图
- external: 第三方或外部插件
- entry/hot: 模块入口文件
- typing: TS模块接口定义文件
- template: 模板文件


## 常见问题

### 如何引入JS/JSX模块
> 解决方案有以下方法：

- 模块通过npm安装，需要编写接口定义文件（d.ts），vscode/webstorm会根据接口文件（d.ts）定义自动映射到模块
- 配置参数：allowJs: true。在tsconfig.json/jsconfig.json中配置
- 模块不通过npm 安装，而是直接从某个文件夹引入JSX，需要由babel先编译再由ts编译（临时解决方案），在ts导入时，需要按照以下方法：（不推荐此类方式，假如引入的模块不存在无法处理的需求，请不要使用该方案）

```
// 导入模块
import calendar = require('../../external/calendar');
// 权宜之计，因为无法识别JSX元素，JSX仅能通过Import from 引入
const Calendar: any = calendar as any;
```

### Import / Require 区别
`import mod from 'modules'` 

> 当模块是ES6时，并且提供default，则使用这个方法（假如该代码被babel编成ES5，无法使用该方法）

```
module.export['default'] = mod;
module.export.default = mod
export default mod
```

`import mod = require('modules')`

> 当模块非ES6，并且未提供default，则使用该方法

```
module.exports = mod;
```

### JSX出现红色下划线，不能够识别为JSX元素
>  满足JSX条件，有以下情形：

- JSX组件首字母必须是大写字母，如 `<ComponentButonn />`。错误的写法：`<componentButton />`
- 通过 Import mod from 'module' 导入， 否则只能强制转换成 any
- 当前文件扩展，必须是tsx
- 必须是Stateless Component / extend React.Component / createElement / createClass 中的一种


## 小技巧 

- tsd.d.ts中引用node_modules/typescript/lib/lib.es6.d.ts，能够确保项目支持ES6语法

- namespace（全局绑定，直接就引用，不需要require） 
- module（基于amd/cmd模式的引用，会export出代码）


- exports.__esModule = true; //babel使用

- ES5导入外部模块（declare module）JS后缀的文件，外部模块提供export default导出当前模块时，使用Import something = ‘sorting’. 否则会带出object.default,同时写d.ts时需要export default导出模块
- ES5本身写的ts是可以直接import something from ‘something’，不会带出 object.default


## TS语法

### 基本类型

Boolean

```
let isDone: boolean = false;
```
Number

```
let binary: number = 0b1010;
let octal: number = 0o744;
```

String

```
let color: string = 'blue';
```

Array

```
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

Tuple（不相同的元素）

```
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

Enum

```
enum Color {Red, Green, Blue};
enum Color {Red = 1, Green = 2, Blue = 4};
enum Color {Red = 1, Green, Blue};
let c: Color = Color.Green;
```

Any

```
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)
let prettySure: Object = 4;
prettySure.toFixed(); // error 
let list: any[] = [1, true, "free"];
```
Void

```
function warnUser(): void {
    alert("This is my warning message");
}
// 只允许设置 undefined / null
let unusable: void = undefined; 
```

类型断点的两种方法

```
// 一般
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

// as 语法
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

### 变量
- `var`: 定义变量
- `let`: 定义块变量 
- `const`: 定义常量
- `Destructuring`: 解构函数

```
let input = [1, 2];
let [first, second] = input;
function f([first, second]: [number, number]){ }
let [first, ...rest] = [1, 2, 3, 4];
let [, second, , fourth] = [1, 2, 3, 4];
let o = {
    a: "foo",
    b: 12,
    c: "bar"
}
let {a, b} = o;
let {a, b}: {a: string, b: number} = o;
// 函数声明
type C = {a: string, b?: number}
function f({a, b}: C): void {
    // ...
}
function f({a, b} = {a: "", b: 0}): void { ... }
function f({a, b = 0} = {a: ""}): void {
    // ...
}
```

### Interfaces(接口）

- 对象

```
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```

- 接口类型转换

```
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
```

- Function

```
interface SearchFunc {
    (source: string, subString: string): boolean;
}
```

- Indexable Types

```
interface NumberDictionary {
    [index: string]: number;
    length: number;    // ok, length is a number
    name: string;      // error
}
```

- Class Types

```
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}
interface ClockInterface {
    tick();
}
class Clock implements ClockConstructor {
    currentTime: Date;
    constructor(h: number, m: number) { }
}
```
- Extending Interfaces（继承接口）

```
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
```

- Hybrid Types（混合类型）

```
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}
```
- Interfaces Extending Classes（接口继承类）

```
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}
```

### Classes（类）

```
// 普通类
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
// 继承
class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}
// public 全局 / protected 不能被外部使用 / private 私有
class Animal {
    protected name: string;
    public constructor(theName: string) { this.name = theName; }
    private move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
// Accessors 
class Employee {
    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}

// static 静态变量，在实例间共享，对于所有实例都是通用的

class Grid {
    static origin = {x: 0, y: 0};
    calculateDistanceFromOrigin(point: {x: number; y: number;}) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor (public scale: number) { }
}
var grid1 = new Grid(1.0);  // 1x scale
var grid2 = new Grid(5.0);  // 5x scale

// Abstract Classes 抽象类 与接口类似，同样不能够被实例化

abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earth...');
    }
}
```

### Functions （函数）

```
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function(x: number, y: number): number { return x+y; };

let myAdd: (baseValue:number, increment:number) => number =
    function(x: number, y: number): number { return x + y; };

function buildName(firstName = "Will", lastName: string) {
    return firstName + " " + lastName;
}

// Rest Parameters

function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}

function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}

let buildNameFun: (fname: string, ...rest: string[]) => string = buildName;


// 重载

function pickCard(x: {suit: string; card: number; }[]): number;
function pickCard(x: number): {suit: string; card: number; };
function pickCard(x): any {
    // Check to see if we're working with an object/array
    // if so, they gave us the deck and we'll pick the card
    if (typeof x == "object") {
        let pickedCard = Math.floor(Math.random() * x.length);
        return pickedCard;
    }
    // Otherwise just let them pick the card
    else if (typeof x == "number") {
        let pickedSuit = Math.floor(x / 13);
        return { suit: suits[pickedSuit], card: x % 13 };
    }
}

let myDeck = [{ suit: "diamonds", card: 2 }, { suit: "spades", card: 10 }, { suit: "hearts", card: 4 }];
let pickedCard1 = myDeck[pickCard(myDeck)];
alert("card: " + pickedCard1.card + " of " + pickedCard1.suit);

let pickedCard2 = pickCard(15);
alert("card: " + pickedCard2.card + " of " + pickedCard2.suit);

```

### Generics（泛型）

```
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);  // Error: T doesn't have .length
    return arg;
}

function loggingIdentity<T>(arg: T[]): T[] {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: <T>(arg: T) => T = identity;

// 也可使用不同泛型表示，只要序列一致
function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: <U>(arg: U) => U = identity;

// 也可以这么使用
let myIdentity: {<T>(arg: T): T} = identity;

// 泛型接口
interface GenericIdentityFn {
    <T>(arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;

// 泛型类
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}
// 泛型约束
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);  // Now we know it has a .length property, so no more error
    return arg;
}

loggingIdentity({length: 10, value: 3});

function find<T>(n: T, s: Findable<T>) {
  // ...
}

// 泛型类约束
function create<T>(c: {new(): T; }): T {
    return new c();
}

class BeeKeeper {
    hasMask: boolean;
}

class ZooKeeper {
    nametag: string;
}

class Animal {
    numLegs: number;
}

class Bee extends Animal {
    keeper: BeeKeeper;
}

class Lion extends Animal {
    keeper: ZooKeeper;
}

function findKeeper<A extends Animal, K> (a: {new(): A;
    prototype: {keeper: K}}): K {

    return a.prototype.keeper;
}

```
### Enum 枚举

```
// 普通枚举
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}
// 更多枚举类型
enum FileAccess {
    // constant members
    None,
    Read    = 1 << 1,
    Write   = 1 << 2,
    ReadWrite  = Read | Write,
    // computed member
    G = "123".length
}
// 返回自身
enum Enum {
    A
}
let a = Enum.A;
let nameOfA = Enum[Enum.A]; // "A"
// 计算
const enum Enum {
    A = 1,
    B = A * 2
}
```

### 类型

```
// 类型合并 
function padLeft(value: string, padding: string | number) {
    // ...
}

// 类型对象合并，只能合并相同的函数或值，不相同的则报错
interface Bird {
    fly();
    layEggs();
}

interface Fish {
    swim();
    layEggs();
}

function getSmallPet(): Fish | Bird {
    // ...
}

let pet = getSmallPet();
pet.layEggs(); // okay
pet.swim();    // errors

// 正确用法 判断类型
let pet = getSmallPet();

// Each of these property accesses will cause an error
if (pet.swim) {
    pet.swim();
}
else if (pet.fly) {
    pet.fly();
}
// 或者强制转换
let pet = getSmallPet();

if ((<Fish>pet).swim) {
    (<Fish>pet).swim();
}
else {
    (<Bird>pet).fly();

// 类型约束或保护
function isFish(pet: Fish | Bird): pet is Fish {
    return (<Fish>pet).swim !== undefined;
}

// typeof

function isNumber(x: any): x is number {
    return typeof x === "number";
}

// instanceof
if (padder instanceof SpaceRepeatingPadder) {
    padder; // type narrowed to 'SpaceRepeatingPadder'
}
// Type Aliases
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
type Container<T> = { value: T };
type Tree<T> = {
    value: T;
    left: Tree<T>;
    right: Tree<T>;
}
// 交叉类型
type LinkedList<T> = T & { next: LinkedList<T> };
interface Person {
    name: string;
}

var people: LinkedList<Person>;
var s = people.name;
var s = people.next.name;
var s = people.next.next.name;
var s = people.next.next.next.name;
// 字符类型
type Easing = "ease-in" | "ease-out" | "ease-in-out";
// 重载
function createElement(tagName: "img"): HTMLImageElement;
function createElement(tagName: "input"): HTMLInputElement;
// ... more overloads ...
function createElement(tagName: string): Element {
    // ... code goes here ...
}
// 多态
let v = new BasicCalculator(2)
            .multiply(5)
            .add(1)
            .currentValue();
```
### Iterators（迭代器） and Generators

```
// for of
let someArray = [1, "string", false];

for (let entry of someArray) {
    console.log(entry); // 1, "string", false
}
```

### Modules

```
// export 各种类型
export interface StringValidator {
    isAcceptable(s: string): boolean;
}
export const numberRegexp = /^[0-9]+$/;
export class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}
export { ZipCodeValidator };
export { ZipCodeValidator as mainValidator };
// rename
export {ZipCodeValidator as RegExpBasedZipCodeValidator} from "./ZipCodeValidator";
// export 所有模块
export * from "./ZipCodeValidator";  // exports class ZipCodeValid
// import
import { ZipCodeValidator } from "./ZipCodeValidator";
import { ZipCodeValidator as ZCV } from "./ZipCodeValidator";
import * as validator from "./ZipCodeValidator";
import "./my-module.js";
// default
export default class ZipCodeValidator 
import validator from "./ZipCodeValidator";

// export = and import = require() 
// export =
export = ZipCodeValidator;
import zip = require("./ZipCodeValidator");

// 动态模块
// for nodeJS
declare function require(moduleName: string): any;
import { ZipCodeValidator as Zip } from "./ZipCodeValidator";
if (needZipValidation) {
    let ZipCodeValidator: typeof Zip = require("./ZipCodeValidator");
    let validator = new ZipCodeValidator();
    if (validator.isAcceptable("...")) { /* ... */ }
}
// for requireJS
declare function require(moduleNames: string[], onLoad: (...args: any[]) => void): void;

import { ZipCodeValidator as Zip } from "./ZipCodeValidator";

if (needZipValidation) {
    require(["./ZipCodeValidator"], (ZipCodeValidator: typeof Zip) => {
        let validator = new ZipCodeValidator();
        if (validator.isAcceptable("...")) { /* ... */ }
    });
}

// 与其他JavaScript库工作
declare module "url" {
    export interface Url {
        protocol?: string;
        hostname?: string;
        pathname?: string;
    }

    export function parse(urlStr: string, parseQueryString?, slashesDenoteHost?): Url;
}

declare module "path" {
    export function normalize(p: string): string;
    export function join(...paths: any[]): string;
    export var sep: string;
}
/// <reference> node.d.ts and then load the modules using import url = require("url");
/// <reference path="node.d.ts"/>
import * as URL from "url";
let myUrl = URL.parse("http://www.typescriptlang.org");
```
### Namespaces

```
// merge namespace
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }
}
/// <reference path="Validation.ts" />
namespace Validation {
    const numberRegexp = /^[0-9]+$/;
    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}
// 别名
namespace Shapes {
    export namespace Polygons {
        export class Triangle { }
        export class Square { }
    }
}
import polygons = Shapes.Polygons
let sq = new polygons.Square(); // Same as "new Shapes.Polygons.Square()"
// 环境名称空间 （d.ts)
declare namespace D3 {
    export interface Selectors {
        select: {
            (selector: string): Selection;
            (element: EventTarget): Selection;
        };
    }

    export interface Event {
        x: number;
        y: number;
    }

    export interface Base extends Selectors {
        event: Event;
    }
}

declare var d3: D3.Base;
```
### Module Resolution

```
// /root/src/folder/A.ts
import { b } from "./moduleB" 
// 会查找
/root/src/folder/moduleB.ts
/root/src/folder/moduleB.d.ts

// /root/src/folder/A.ts
import { b } from "moduleB"  
// 查找以下文件
/root/src/folder/moduleB.ts
/root/src/folder/moduleB.d.ts
/root/src/moduleB.ts
/root/src/moduleB.d.ts
/root/moduleB.ts
/root/moduleB.d.ts
/moduleB.ts
/moduleB.d.ts

```

### Declaration Merging

```
// Merging Interfaces
interface Document {
    createElement(tagName: any): Element;
}
interface Document {
    createElement(tagName: "div"): HTMLDivElement;
    createElement(tagName: "span"): HTMLSpanElement;
}
interface Document {
    createElement(tagName: string): HTMLElement;
    createElement(tagName: "canvas"): HTMLCanvasElement;
}
// Merging Namespaces
namespace Animal {
    let haveMuscles = true;

    export function animalsHaveMuscles() {
        return haveMuscles;
    }
}

namespace Animal {
    export function doAnimalsHaveMuscles() {
        return haveMuscles;  // <-- error, haveMuscles is not visible here
    }
}
// Merging Namespaces with Classes, Functions, and Enums
enum Color {
    red = 1,
    green = 2,
    blue = 4
}

namespace Color {
    export function mixColor(colorName: string) {
        if (colorName == "yellow") {
            return Color.red + Color.green;
        }
        else if (colorName == "white") {
            return Color.red + Color.green + Color.blue;
        }
        else if (colorName == "magenta") {
            return Color.red + Color.blue;
        }
        else if (colorName == "cyan") {
            return Color.green + Color.blue;
        }
    }
}
// 全局扩展
// observable.ts
export class Observable<T> {
    // ... still no implementation ...
}

declare global {
    interface Array<T> {
        toObservable(): Observable<T>;
    }
}

Array.prototype.toObservable = function () {
    // ...
}
```
### Mixins

```
class SmartObject implements Disposable, Activatable

applyMixins(SmartObject, [Disposable, Activatable]);

function applyMixins(derivedCtor: any, baseCtors: any[]) {
    baseCtors.forEach(baseCtor => {
        Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => {
            derivedCtor.prototype[name] = baseCtor.prototype[name];
        });
    });
}
```
## 参考链接

- [Typescript官网](http://www.typescriptlang.org/)
- [Wiki](https://github.com/Microsoft/TypeScript/wiki)
- [编译选项](https://github.com/vilic/typescript-guide/blob/master/wiki/compiler-options.md)
- [TypeScript 新增特性一览](https://github.com/vilic/typescript-guide/blob/master/wiki/whats-new-in-typescript.md)