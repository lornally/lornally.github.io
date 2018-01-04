> 源码地址: https://github.com/rauchg/smashingnode

# 第一部分: 建立概念

### 第一章 安装

1. mac 下面直接用brew是最方便, brew不会用参考隔壁文章.\
2. terminal下面, node可以直接进入repl模式, 很不错, 和浏览器的console模式差不多. 但是, 更轻盈更舒适. 
3. 执行: node xxx.js
4. 包安装: cnpm xxx
5. package.json规定了包的内容, 有了package.json, 用cnpm install就可以自动安装指定的依赖包.

```sh
brew install node #安装node
node #直接进入repl模式
node xxx.js #执行xxx.js
npm install -g cnpm --registry=https://registry.npm.taobao.org #先安装cnpm
cnpm install xxxx #用cnpm安装xxxx包
```

第一个js

```javascript
//彩虹字的示例代码, 这个值得学习. 短短两行, 就可以爽到.
require('colors')
console.log("smashing node".rainbow) //这里要关注下, 点rainbow, 很明显rainbow是一个属性, 而是是建立在字符串原型上面的属性, 这种代码看着就爽.
```

package.json

```json
{
  "name":"myp" //包名
  , "version":"0.0.1" //版本
  , "dependencies":{ //依赖包
    "colors": "1.1.*"
  }
  , "main": "./index" //入口文件
  , "private": "true" //这个代表不会发布到公开源比如npm
  ,"bin": "./bin的目录" //发布二进制包, 就是那种安装的时候用-g的
 }
```

安装二进制工具包

```sh
npm install -g express #安装了express这个命令行工具包.
```

### 第二章 js概览

1. 多用字面量, 别用new.

```js
//函数老有趣了. 
var a = function b(x, y, z){
 	typeof a //'function'
 	typeof b //'undefined', b不存在于外部调用域, 仅仅是递归调用时使用. 而函数名的作用域在于函数外部, 因此此处是undefined
 }
a.length //3, 这个是函数的参数个数.
```

2. 闭包, 尽量别用. 用let解决作用域问题, let的作用域是{大括号}
3. prototype也尽量别用, 其实prototype唯一的用处是扩展系统类型. 
   1. 但是这么干却很容易造成冲突.
   2. 而且会形成框架式写法, 会改变语法, 就不是包的写法了.
   3. 因此, 这种写法叫monkeycode(类似magiccode一样的贬义词).
4. try catch, 这个也不建议随便用.
5. v8优势

```js
// key-value de key
var c={ 
	"x":"xxx"
	,"y":"yyy"
}
for(var i in c){}//这个是一般的写法.
Object.keys(c); //['x','y'], 这个是v8写法.

//array
Array.isArray(c) //false, 判断a是否是数组.
[1,2,3].forEach(这里是回调函数) //数组的另一个遍历方法

//JSON
JSON.parse('{"a":"b"}') //JSON.stringify(a), 把对象序列化为json字符串.

//函数
var v=a.bind({}); //对于a的this做替换, 实例化一个function a, 但是, 他们的this指向不同.
a.name //'b', 这个有趣了, 显示的是a的函数名, 咱们刚刚说了, a的函数名在函数体之外是不可见得.
//因为这个特性是为了跟踪错误的, 因此, 建议始终对函数命名, 不要使用匿名函数.

//__proto__ 定义继承链.
xxx.prototype.__proto__=ooo.prototype

//用属性代替属性方法, 调用的时候, 可以避免(小括号).
Date.prototype.__defineGetter__('ago', xxxx)//这里xxxx是一个函数.
var a =new Date();
a.ago //看到了, 这个地方就不需要a.ago()了, 为了省一个括号, 各路大神也是蛮拼的.
```

6. 始终保持对函数命名(利于调试).

> 感悟: 好语言和差语言之间的差异, 可能就在一个括号, 或者一个分号上面. 日心说比地心说先进, 就在于日心说简洁明了7个椭圆, 地心说要30+圆, 简洁就是正确.

最后一个段落, 翻译没说人话, 这个其实不是翻译的锅, 因为翻译很容易掉到沟里面, 这个编辑的锅, 我的翻译:

> 理解本章可以迅速掌握javascript本质, 利于理解javascript语言中那些怪异的部分,  他们主要是由于残破的运行环境(例如IE)造成
>
> 完全倒装的一种翻译: 
>
> 这个语言传统上的运行环境都比较残破, 比如老旧的各种浏览器环境, 由此导致了语言中有各种怪异的部分, 掌握本章有助于拨开迷雾, 了解到语言的本质.
>
> 牛逼的翻译, 可以把一大厚本, 翻译成一个小薄本, 中文本身就简洁, 理应比原文薄.

### 阻塞与异步回调

```js
xxx.on('error', callback)
xxx.on('UNcaughtexception', callback)
//还有回调函数的第一个参数也是error.
//在js里面, 看到error, 就要优先处理它, 因为, 异步调用栈会丢失掉上下文信息, 如果不处理, 就没机会处理了.
```

### node中的js

全局

```js
//全局
window.name //这是浏览器的全局对象.
global //node的全局对象
process.title //这是node的全局对象.
setTimeout //并不是javascript写的.
```

模块(包)管理

```js
//模块(包)组织的三个核心全局变量:
require
module
exports
require('./xxx.js')//引入一个本地javascript文件.
require('fs')//引入一个包. package.json中name指定的.
exports.xxx //暴露xxx属性给外部require引用.exports其实是对module.exports的引用. 
module.exports = Person;
function Person (name) {//这么干, 别人require拿到的就是一个function了.
  this.name = name;
};
```

事件

```js
EventEmitter
window.addEventListener
process.EventEmitter
//在js里面, 你可以在任何地方监听或者发出时间event.
var EventEmitter = require(‘events’).EventEmitter
var EventEmitter = process.EventEmitter //process保持了一个对于eventemitter的引用. 奶奶的. 才明白.

//一般情况
xxx.on('某个事件', 回调函数)
//只执行一次
xxx.once('某个事件', 回调函数)
//问题来了, xxx究竟支持哪些事件呢? 查手册.......
```

buffer

```js
//二进制处理, 哈哈js也有直接写存和直接写屏了啊.......
```

# 第二部分 API

### 命令行工具和fs

使用package.json是个好习惯

```json
{
  "name": "myp",
  "version": "0.0.1",
  "dependencies": {
    "colors": "1.1.*"
  },
  "main": "./index",
  "private": "true",
  "description": "第一个nodejs程序, 之前那个拉力测试不算, 因为那个的任务栈会溢出, 其实这个colors的依赖没啥用"
}
```

```sh
npm install #这个可以测试package.json是否正确.
```

读取文件

```js
fs.readdir(_dirnamce, listfile)
```

流

```js
//console.out就是对process.stdout做了包装(行尾加了/n回车).
process.stdout.write()
process.stdin //默认是pause状态
process.stderr
stream 是一个eventEmitter
```

糟粕

```js
process.cwd()// returns the current working directory, i.e. the directory from which you invoked the node command.
__dirname //returns the directory name of the directory containing the JavaScript source code file
//疯了, 彻底疯了, 参数, 全局变量和函数名, 这哥们都用同一个, 神经啊........ 这本书的第一个坑, 我就是这么踩进去的.
---------------------------神经和正常的分割线---------------------------
'.'//用.多好? 上面这两个全局变量太神经了.
```

改进

```js
//变量, 参数, 函数名我都改为不同的名字了.
var log=console.log  //这个用了很多次, 因此, 我定义了一个变量
```

服了

> 真的是大神, 在本章末尾, 大神解释了我之前不明白的一切. 哦, 这种书写方式......