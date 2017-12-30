> 源码地址: https://github.com/rauchg/smashingnode

### 第一章安装

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
 }
```

