> codemirror1是文档优良的典范, 不仅仅有story介绍项目的来龙去脉, 技术方案, 心路历程, 而且有manual, 这个manual也是值得一读的.

这个manual不仅仅介绍了如何使用codemirror, 而且介绍了codemirror的模式, 尤为重要的是详细解释了项目中的功能分布. 

###### codemirror的使用和模式

```js
var editor = new CodeMirror(document.body, {
  parserfile: "parsexml.js",  //这个是使用的parse
  stylesheet: "xmlcolors.css"  //这个是使用的css
});
```

- 在codemirror后续版本中, 对于这一个地方进行了重大改进, 改为一个配置文件. 

###### codemirror的功能分布和文件组织方式

- codemirror.js 接口文件, 使用codemirror时引用这个.
- editor.js 负责用户输入, 高亮代码, 行缩进.
- util.js 通用函数
- undo.js 处理回退历史
- stringstream.js 包装文本输入给parser
- select.js 处理选择和光标
- tokenize 写token的框架

###### 文档后面的内容也很多

尤其是对接口的介绍很详尽.

###### 阅读源码的感想

cm1里面大量的代码其实都是为了抹平浏览器差异的公共接口抽象代码. 