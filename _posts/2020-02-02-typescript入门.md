> 太多人建议ts了, 趁着假期, 入门一下.

#### 要点列一下:

- 在构造函数的参数上使用`public`等同于创建了同名的成员变量。
- tsconfig.json可以设置各种参数, 
  - 最重要的是: sourceMap:true, 这个参数让ts可以直接被调试
  - 混合js的参数是: allowJs:true
- 在vs code里面调试ts
  - 必须set `"sourceMap": true` in tsconfig.json 
  - 必须设置 `outFiles` in  launch.json

#### 配置文件

tsconfig.json

- 位置: 项目根文件

```json
{
  "compilerOptions": {
    "target": "es5",    //指定编译目标es5
    "module": "commonjs",  // 指定module
    "outDir": "out",    //指定输出文件夹
    "sourceMap": true   //  指定调试文件
  }
}
```

tasks.json

- 位置: .vscode目录

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "type": "typescript",
            "tsconfig": "tsconfig.json",
            "problemMatcher": [
                "$tsc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```

code>preferences>settings

```json
//这里可以设置忽略那些编译出来的js文件, 比如
"**/*.js": { "when": "$(basename).ts" }, 
"**/**.js": { "when": "$(basename).tsx" }  //key不能相同, 所以用了**.js, 这是一个trick

//还可以设置workspace
  "typescript.tsdk": "/usr/local/lib/node_modules/typescript/lib"
```

launch.json

- 位置: .vscode目录
- 快捷键: shift + cmd + D

```js
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
      "program": "${workspaceFolder}/helloworld.ts",
      "preLaunchTask": "tsc: build - tsconfig.json",
      "outFiles": ["${workspaceFolder}/out/**/*.js"]
    }
  ]
}
```



#### 不错的入门文档:

- 必须先看官方文档, 简历基础的感知, 官方文档很明确, ts的类型是为了接口定义而做的.

  - https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html
  - 概览  https://code.visualstudio.com/docs/typescript/typescript-tutorial
  - 编译  https://code.visualstudio.com/docs/typescript/typescript-compiling
  - 调试  https://code.visualstudio.com/docs/typescript/typescript-debugging
  - react实践: https://zhuanlan.zhihu.com/p/34285874
  - ts的设计目标 https://github.com/Microsoft/TypeScript/wiki/TypeScript-Design-Goals
- https://ts.xcatliu.com/basics/type-assertion

  - 简单清晰明了.
  - 最后的工程部分值得一读.
  - 总阅读时长大约4小时.
  - 深入理解ts,  https://jkchao.github.io/typescript-book-chinese/project/compilationContext.html#tsconfig-json
- 关于泛型: https://juejin.im/post/5d0b7c03f265da1b8811e265

  - <T>:表示范型，具体什么类型是调用的这个方法的时候决定的（注意：T也可以是任意其他字母） 
  - 这个文档讲了命名原因Type Parameter Naming Conventions  : https://docs.oracle.com/javase/tutorial/java/generics/types.html
- https://zhongsp.gitbooks.io/typescript-handbook/content/index.html

  - 官方手册的中文版
  - 是其他几篇入门文章作者的参考文档.
  - 这个可以当手册查, 不过官方的更合适. https://juejin.im/post/5c0a11e3e51d456ff54c09aa


