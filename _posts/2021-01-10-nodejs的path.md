> node的path还是有一点内容的, 其中两个主要的拿当前目录的方法有微妙的区别

###### 本地目录

```sh
process.cwd() # 简单说 process.cwd()是一般情况, 他是执行命令的地方
# returns the current working directory,i.e. the directory from which you invoked the node command.
__dirname # __dirname的使用面会小一点, 因为他是源代码所在地例如src/, 有时候会需要使用这个, 比如调用安装目录下的某些模块.
# returns the directory name of the directory containing the JavaScript source code file
```

###### 系统用户根目录 ~

```sh
os.homedir() # 对应 ~
~/xxx # 这样是OK的
"~/xxx" # 这样就不对了, 因为引号之内的~不会被处理为用户home目录
```

###### 拿到绝对目录

```coffeescript
cmdpath= pa.resolve(__dirname, 'dist/api')
```

###### 拿到某个path的目录部分, 并顺手递归的建立这个目录

```coffeescript
import pa from 'path'
fs.mkdir (pa.dirname path),recursive:true
```

###### 这个递归创建目录的函数, 完美的解释了dirname是啥

```js
var path = require('path'),
    fs = require('fs');
function ensureDirectoryExistence(filePath) {
  var dirname = path.dirname(filePath);
  if (fs.existsSync(dirname)) {
    return true;
  }
  ensureDirectoryExistence(dirname);
  fs.mkdirSync(dirname);
}
```

