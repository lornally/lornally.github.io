# 要点 -- 2023.2.22

## 配置

```json
// vscode配置一个launch.json
{"version": "0.2.0",
 "configurations": [{
   "type": "node",
   "request": "launch",
   "name": "Launch Program",
   "skipFiles": [
    "<node_internals>/**"
   ],
   "program": "${file}", // 当前文件
   "outFiles": [ "${workspaceFolder}/dist/cmd/*.js"], // 核心配置 
   "runtimeArgs": ["--experimental-modules"] //核心配置是这一句 
 }]}
```

## 运行

```sh
# package.json 配置这个
"scripts": {
    "watch": "webpack --watch ",
# terminal执行这个
yarn watch
```





# 原本的细节

###### 配置launch.json

```js
//最终的版本很精简
{
  version: '0.2.0',
  configurations: [
    {
      type: 'node',
      request: 'launch',
      name: 'Launch Program',
      skipFiles: ['<node_internals>/**'],
      program: '${file}',
      outFiles: [
        '${workspaceFolder}/dist/api/*.js'
      ]
    }
  ]
}

//解释
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
    "configurations": [
      {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        // "runtimeArgs": ["--experimental-modules"], //支持module核心配置是这一句, 但是现在也不需要了.
        "program": "${file}", //这个好, 这个是当前文件
        "skipFiles": [
          "<node_internals>/**"
        ],
        // "sourceMaps": true, //这个没必要, 本来默认就是true
        //"stopOnEntry": true, //这个并不是必须的
        //smartStep": false, //也不是必须的
        //下面三行是关键内容
        "outFiles": [ //这个字段很关键, 没有这个就不能调试coffee, 这是说到哪里去搜索
          "${workspaceFolder}/dist/api/*.js"
          //"这里是webpack输出的js的地址", 配置成下面这样也可以, 搜索目录会多
          //"${workspaceFolder}/**/*.js"
        ]
      }
    ]
}
```

###### webpack

- 打开watch
- 输出文件名

```coffeescript
# package.json设置: 
"scripts": {
  "watch": "webpack --watch --config webpack.dev.js"# 可以是简明的 webpack --watch, 不一定要指定配置config文件
},

# webpack 配置文件
#filename: '[name].[contenthash].js' # 这个会导致vscode的调试不生效. 好神奇.
filename: '[name].js' # 这个就没有问题了.
```

###### 使用glob指定多个文件

```coffeescript
# package.json
yarn add webpack-glob --dev 
"devDependencies": {   
    "webpack-glob": "^2.0.2", 
  },
# webpack配置文件
glob = require "glob"
# ...
entry:{
		# bin: './src/test.cs' #直接指定测试文件, 是比较柔软的做法
		# 这个要指明目录,web专属内容cmd是编译不过去的, 比如css module
		(glob.sync('./src/**/**.cs').reduce (obj, el)->
			obj[path.parse(el).name] = el
			obj
		,{})...
	}
```

###### 特别提示

```
# package.json的module设置是没用的
# 不止没用, 还很可能捣乱, 引起webpack不正常, 因此最好删除掉, package.json是纯正的json, 不能有注释
"type": "module",
```

### 成功和失败

- 点虫子图标就可以了. 
- 文件名右键之前不行
  - 不论是tab的文件名
  - 还是左边资源管理器的文件名
  - 后来可以了, 可能是之前没有生成对应的js文件的原因
- 从左边选虫子三角肯定不行, 

### watch 一定要打开

- 改了文件一定要webpack重新编译, 因此应该用watch

## 问题

###### import http或者fs的时候, debug报错

```sh
# Uncaught ReferenceError: require is not defined

神啊, 解决方案是package.json里面删除type: module
```

- 之前的记录没有粗, 确实不能有type:module

### 这些没有成功, 跑偏了.

###### 执行

```sh
# 此项目已经废弃 https://github.com/node-inspector/node-inspector
# npm install -g node-inspector
# coffee -c -m *.coffee
# node-debug app.js
# 他的替代版本: 
node --inspect index.js
# 官方说用这个, 其实也不灵.
coffee --nodejs debug server.coffee
```



###### 报错

```sh
# 第一个错
/usr/local/bin/node ./src/mcktools/test.js
Could not read source map for coffeescript: Invalid URL: coffeescript
(node:27427) Warning: To load an ES module, set "type": "module" in the package.json or use the .mjs extension.
(Use `node --trace-warnings ...` to show where the warning was created)
Process exited with code 1

# 第二个错
Unknown file extension ".cs"
# 这个错误就是 package.json的 'type':'domule'引起的



# 报错
node src/mcktools/test.cs   # 下面是报错:
1 ↵
(node:3383) Warning: To load an ES module, set "type": "module" in the package.json or use the .mjs extension.
(Use `node --trace-warnings ...` to show where the warning was created)
/Users/machangkun/git/work/weixin/天眼/tianyanbackstage/src/mcktools/test.cs:5
import router from './router.cs'
^^^^^^

SyntaxError: Cannot use import statement outside a module
		# 改package.json	添加type:madule
		# 后来证明这个是错误做法

# 第二个错
# 这个错误就是 package.json的 'type':'domule'引起的
node src/mcktools/test.cs    # 下面是报错:                                                                                                                                                                                                1 ↵
internal/process/esm_loader.js:74
    internalBinding('errors').triggerUncaughtException(
                              ^

TypeError [ERR_UNKNOWN_FILE_EXTENSION]: Unknown file extension ".cs" for /Users/machangkun/git/work/weixin/天眼/tianyanbackstage/src/mcktools/test.cs

# 这样还是一样的
node --inspect  src/mcktools/test.cs                                                                                                                                                                                                                                                  1 ↵
Debugger listening on ws://127.0.0.1:9229/e764f1a4-2c47-4ea1-8bc0-481266a0f31e
For help, see: https://nodejs.org/en/docs/inspector
internal/process/esm_loader.js:74
    internalBinding('errors').triggerUncaughtException(
                              ^

TypeError [ERR_UNKNOWN_FILE_EXTENSION]: Unknown file extension ".cs" for /Users/machangkun/git/work/weixin/天眼/tianyanbackstage/src/mcktools/test.cs

# 换这一句, 是被废弃的
coffee --nodejs debug  src/mcktools/test.cs

# 换这一句
coffee --nodejs inspect  src/mcktools/test.cs # 反馈正常了一点, 但是依旧不对劲
< Debugger listening on ws://127.0.0.1:9229/04eda494-505a-4cba-8f71-6bb4a6825143
< For help, see: https://nodejs.org/en/docs/inspector
< Debugger attached.
Break on start in /usr/local/lib/node_modules/coffeescript/bin/coffee:4
  2 
  3 try {
> 4   new Function('var {a} = {a: 1}')();
  5 } catch (error) {
  6   console.error('Your JavaScript runtime does not support some features used by the coffee command. Please use Node 6 or later.');

```

###### 可能是解决方案

```js
//写一个入口js
CoffeeScript = require('coffee-script');
CoffeeScript.register();
require('./src/main');
```

## 参考

###### 有用

- vscode debug coffee 很简单:  https://github.com/Microsoft/vscode/issues/5963
- package.json不能有type:module, 
  - https://stackoverflow.com/questions/31931614/require-is-not-defined-node-js
  - https://stackoverflow.com/questions/63386054/typeerror-err-unknown-file-extension-unknown-file-extension-ts-on-visual-s
  - https://communicode.io/how-to-fix-unknown-file-extension-error-node-typescript/
  - https://learn.coderslang.com/0021-nodejs-require-is-not-defined-error/
- 当年自己写的答案
  - https://stackoverflow.com/questions/46239018/debugging-coffeescript-in-vscode-goes-to-js-files



###### 官方

- vscode官方文档: 
  - https://code.visualstudio.com/docs/nodejs/nodejs-debugging
- node和Chrome V8团队的官方文档:
  - https://medium.com/@paul_irish/debugging-node-js-nightlies-with-chrome-devtools-7c4a1b95ae27#.pmqejrn8q
  - https://nodejs.org/en/docs/guides/debugging-getting-started/
  - https://stackoverflow.com/questions/62096269/cant-run-my-node-js-typescript-project-typeerror-err-unknown-file-extension



###### 低用途

- https://github.com/jashkenas/coffeescript/wiki
- https://stackoverflow.com/questions/4679782/can-i-use-coffeescript-instead-of-js-for-node-js
