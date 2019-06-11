> 在umi中使用es6, 忽然报错: 
> Unhandled Rejection (TypeError): The "original" argument must be of type Function
>  modules[moduleId].call(module.exports, module, module.exports, hotCreateRequire(moduleId));

###### 第一步问高人

- 公司两个同事都没见过这个问题, 也都没有思路. 

###### 第二步 google和stackoverflow

- 有一些人说是node版本问题, 主要再8和12这两个版本玩命, 我装了最新的12.31, 然并卵.
- 另一些人说是webpack和babel的问题, 不要装任何的bable的es6module插件. 我看了, 项目中貌似没有.

###### 第三步 用笨功夫一步一步来吧

- 发现依旧是export的副作用的影响, 只要import了模块, 虽然没用但是, 会导致无法被webpack编译.
- 最终结果貌似是stampit和umi不兼容.  这个结论是错的. 
- 实际的问题是, 项目本身依赖了a和b, a也依赖b, 这个情况下, 就出问题了.
- 问题在于项目本身依赖的b的版本号和a以来的版本号不一致, 就出现了这个问题. 改为一致, 问题解决.