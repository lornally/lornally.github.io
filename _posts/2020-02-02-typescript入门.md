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

launch.json

#### 不错的入门文档:

- 必须先看官方文档, 简历基础的感知, 官方文档很明确, ts的类型是为了接口定义而做的.

  - https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html
  - 
- https://ts.xcatliu.com/basics/type-assertion

  - 前面讲的简单清晰明了.
  - 最后的工程部分值得一读.
  - 总阅读时长大约4小时.
  - https://ts.xcatliu.com
- https://zhongsp.gitbooks.io/typescript-handbook/content/index.html

  - 官方手册的中文版
  - 是其他几篇入门文章作者的参考文档.
- https://github.com/Microsoft/TypeScript/wiki/TypeScript-Design-Goals

  - ts的设计目标
- https://juejin.im/post/5c0a11e3e51d456ff54c09aa

  - 这个可以当手册查, 不过官方的更合适. 
- 关于泛型可以看这个: 

  - https://juejin.im/post/5d0b7c03f265da1b8811e265
- <T>:表示范型，具体什么类型是调用的这个方法的时候决定的（注意：T也可以是任意其他字母） 
  -  这个文档讲了命名原因Type Parameter Naming Conventions  : https://docs.oracle.com/javase/tutorial/java/generics/types.html
- https://jkchao.github.io/typescript-book-chinese/project/compilationContext.html#tsconfig-json
  -  深入理解ts, 这个不错哦.

