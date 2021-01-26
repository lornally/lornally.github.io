> 了不起的nodejs上面很简单: npm publish

然而实际上这么干不行.

1. 注册一个npm账号

2. 命令行操作

   ```sh
   npm adduser
   npm login
   npm config ls
   ```

3. 然后npm publish 依旧失败: 

   ```sh
   403 Forbidden - PUT https://registry.npmjs.org/ - You do not have permission to publish "". Are you logged in as the correct user?
   ```

4. 万能的stackoverflow说应该是名字重复了.

   ```sh
   npm search mtool
   ```

5. 真的是这样的.npm



###### 20210125 补充后记

- main属性很有用, 他指明了入口. 那么如何定义多个入口entry呢? 使用exports

```json
//package.josn这么写 
"main": "dist/index.js",
  "exports": {
    ".": "dist/index.js",
    "./cmd": "dist/nodejs",
    "./web": "dist/web",
  }

//引入的时候这么写:
import {file} from 'mcktools/cmd'
```

- 令人发指的是, npm要求10位密码. 神啊, 没有keychain这怎么记得住

- .npmignore很有用, 

  - 很多时候不能依赖gitignore, 
  - 并且需要注意的是, 如果加入了.npmignore, 那么npm会忽略gitignore, 如果我们使用了webpack, 这恰好是我们需要的特性.
  - 因为webpack的输出目录, 恰好是需要gitignore这些输出目录, 但是, 这些输出目录又恰好是npm需要的目录.

  ```sh
  # 因为依赖webpack的输出, 因此这些目录和文件都是可以忽略的
  src
  webpack.**
  yarn
  .vscode
  .eslintrc.js
  coffeelint.json
  postcss.config.js
  babel.config.json
  yarn-error.log
  ```

- 如果像上面这样需要ignore太多目录, 那么其实可以用白名单, package.json的files属性

```
{
  "files": [
    "index.js"
  ]
}
```

- 如何查看npm包含哪些文件呢? 用pack命令

```sh
npm pack
```

- 参考: https://zellwk.com/blog/ignoring-files-from-npm-package/
- 对应git: https://github.com/zellwk/zellwk.com/blob/master/src/posts/2019-04-03-ignore-files-from-npm-package.md