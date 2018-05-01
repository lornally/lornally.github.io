node

- 首先需要有node环境. 
  - 官方建议直接去node官网. 
  - 建议不要用brew, 因为brew是安装在自己的目录环境中, 而很多的node组件写的并不严谨, 会直接去找node默认目录, 因此会出问题. 
  - 我自己用的brew安装, 在运行electron是会有一个冲突, 不过这个冲突是可以忽略的.
- 验证node
  - 在terminal里面
  - node -v
  - npm -v
  - 如果上面两个命令都正常, 那么node就OK了.

开发工具

 - 官方建议
    - vscode 速度更快
    - atom
    - 建议这两个是因为他们对js的支持比较好.

chrome

 - electron支持chrome extending devtools
 - 主要是为了调试各种框架的.
 - 一个忠告: 用包别用框架. 
    - 包和框架的区别在于: 包不改变语法, 不影响你的代码,
    - 比如jquery就是框架, 他抢占了很多变量名, 并且改变了你的代码的写法.
    - 相反node的fs是个包, path也是个包, 你只是用自定义的变量引入了这个包而已.

目录结构

- 一个基础的目录
  - package.json
  - xxx.js
  - xxx.html
- 可以用npm init初始化出来这么一个目录, 哈哈, 我自己从来没这么干过.

打包工具

- 官网介绍了3个:
  - electron-packager
  - electron-forge
  - electron-builder
- 关于打包这事比较复杂, 另外写了blog, 参考隔壁

调试

- electron --inspect=5858 your/app   需要一个调试工具监听5858端口. 哦 atom和vscode在这里有用了.
- --inspect-brk=[port\] 这个意思是碰到错误就pause.
- 专门讲了如何用vscode调试: https://electronjs.org/docs/tutorial/debugging-main-process-vscode
- 目前我没有用到调试, 因此, 就不发表意见了.
- 对于调试, 需要单独的blog来说明.