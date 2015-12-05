# 使用Babel6总结

babel6，较上一版本，做了很多拆分。把babel做成一个容器，拆分原有的自带ES6的功能，更名为ES2015，估计后续会有ES2016等等。导致很多伸手党、百度党等等纷纷中招，以致出现相当多的坑。硬生生的把我缩回到了babel5.8。在此不推荐大伙在生产环境用。

## babel环境安装与配置
首先，Babel6在npm3.0以前体积相当大（约300M左右），因为依赖库重复安装量太大，建议升级node版本至5.1，npm默认就会升级到3.x。（window安装成功后会有相当多的进度条）

第一步，安装核心库

```
npm install babel-core --save-dev
```
第二步，安装预设（presets）库

1、预装ES2015

支持es6语法，具体可看：http://babeljs.io/docs/learn-es2015/

```
npm install babel-preset-es2015 
```
2、安装runtime插件

提供ES6类型扩展，如：array.of、json.stringify、object.assign、symbol、iterator等。相关方法可看：阮一峰 ES6入门：http://es6.ruanyifeng.com/

FAQ：

- transform-runtime 内部引用了 babel-runtime，绝大部分扩展出自于babel-runtime
- babel-runtime引用在babel6下，会有引起symbol/async等扩展库缺失的bug，所以ES6类型扩展在babel6下的bug，目前最新的babel-runtime6.18及其以前是有问题的（2015.12.5） 

```
npm install babel-plugin-transform-runtime babel-runtime
```
3、babel-preset-stage （选择安装）

提供ES7功能

- Stage 0 - 实验阶段
- Stage 1 - 建议阶段
- Stage 2 - 草案阶段
- Stage 3 - 稳定阶段

详情可参考：http://babeljs.io/docs/plugins/#transform

安装了Stage 0 就包含 1-3阶段所有支持的特性。

```
npm install babel-preset-stage-0
```

4、react（选装）

提供react、jsx支持，目前babel5.x以后都支持jsx和react的编译。（jsx已废弃）

```
npm install babel-preset-react
```
5、babel-loader（选装）

webpack-loader支持

```
npm install babel-loader
```
6、node require hook（选装）

- node环境支持使用require来解析babel代码。
- 方法是require('babel-register'). 注意：5.x是require('babel/register')
- 需要安装babel-register(6.x)

7、最后一步，配置当前项目babel环境参数：

- 项目文件夹新建 .babelrc 文件，输入以下内容：

```
{
   "presets": ["es2015", "stage-0", "react"],
   "plugins": ["transform-runtime"]
}
```
stage-0和react请根据项目需要选装


## 让项目跑起来

以下有几种方式可以让项目跑起来

1、命令行（cli模式）：通过终端，进入项目文件夹，全局安装命令行

```
npm install --global babel-cli
babel script.js                 // 编译scripts
babel script.js -out-dir dist   // 编译scripts到dist文件夹
babel-node test.js              // 编译并运行test.js     
```
文档：http://babeljs.io/docs/usage/cli/

2、API

我们在写gulp插件、webpack loader或者node时，需要使用API进行babel编译

```
var babel = require("babel-core");
babel.transform(code, [options]) // => { code, map, ast }
var result = babel.transform("code();", options);
result.code;
result.map;
result.ast;
```

文档：http://babeljs.io/docs/usage/api/

3、Require Hook

提供node require babel编译支持

文档：http://babeljs.io/docs/usage/require/

4、polyfill（不建议）

一般是用到browser（浏览器端），即用script标签引入。babel-node/babel-runtime已经包含了polyfill。不建议采用，因为我们都习惯使用工具（webpack/gulp）编译babel。


## 常见问题

1、gulp支持

```
var gulp = require('gulp');
var babel = require('gulp-babel');

gulp.task('default', () => {
    return gulp.src('src/app.js')
        .pipe(babel({
            presets: ['es2015']
        }))
        .pipe(gulp.dest('dist'));
});
```
参考：https://github.com/babel/gulp-babel
2、webpack支持

```
module: {
  loaders: [
    {
      test: /\.jsx?$/,
      exclude: /(node_modules|bower_components)/,
      loader: 'babel' // 'babel-loader' is also a legal name to reference
    }
  ]
}

```
参考：https://github.com/babel/babel-loader

3、__esModule （默认false)

代表当前模块是es2015输出的模块，当为true，则export default 就不会输出成object.default
 
示例：

```
 add.js
 ---- ---- ----
 exports.__esModule = false;
 const x = 6, y = 8;
 export default x + y
 
 ---- ---- ----
 
  file.js
 ---- ---- ----
   
 const add =  require('./add');
 
 // 调用时须 add.default()，若为true，则 add(); 
 
 // 如果使用import，则不新增default。
 
 import add from './add';
 add();   // 14;
 ---- ---- ----
 
``` 

4、更多参考资料：

- babel实战：https://cnodejs.org/topic/565c65c4b31692e827fdd00c
- babel+webpack+react0.14示例：https://github.com/ruanyf/react-babel-webpack-boilerplate
- react-webpack-babel: https://github.com/alicoding/react-webpack-babel
- koa-react-full-example:https://github.com/dozoisch/koa-react-full-example


最后编辑：2015.12.05


