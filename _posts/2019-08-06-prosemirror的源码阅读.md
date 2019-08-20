> 看了文档之后, 就要再阅读一下源码

###### 



###### 选择了3个版本, 面临同样的问题

- prose20150731-4147883, 选取原因: 文档之后, 20150731
- prose20150624-cde37b7, 选取原因: 文档之前, 20150624
- prose2016-0.1, 选取原因: 第一个释放出来的版本.

问题就是: 这货肿么运行? 官网完全莫名. 去安装了example-setup, 继续搞不懂.

- 回到github, 官方的readme: https://github.com/ProseMirror/prosemirror
- 最后一段, 告诉了我们如何运行demo.

1. 使用yarn, 此处不支持npm.  神啊, 他的example里面为毛不说呢?
2. 然后就可以直接访问html了, 没错, 静态的html. 真心…...

看一下example的版本

- 依旧无法搞定, 完全不明白哦.

再看一下目前的最新版本prosemirror

- 第一步依旧是yarn
- 然后出现了bin目录, 然后可以用这个脚本做事了.
- 此时npm run demo就直接运行了.
- 此时localhost:8080就可以访问了. 
- 这个编辑器就算基本正常吧, 但是, 其实格式的删除还是有很大问题, 貌似这个编辑器的完成度还是有相当大的问题.

搜索了一下, prosemirror的example是著名的看不懂.

- 这里专门说了如何使用: http://hexopress.com/@joe/blog/2019/01/07/how-to-install-use-prosemirror-in-your-project
- 文章里面说的编译目标是/public, 从配置文件看应该是/dist.
- 然后手撸一个html就OK了, 神奇了, 为毛这个example不包含一个html呢? 
- 然后依旧有问题, 需要module.

原本的代码:

```html
<link rel="stylesheet" href="https://prosemirror.net/css/editor.css" />
  <div id="editor" style="margin-bottom: 23px"></div>
<script src="index.js"></script>
<script>
  // window.ProseMirror is active by now.
  // var plugins = ProseMirror.exampleSetup({
  //     schema: ProseMirror.basicSchema
  // });

  // var view = new ProseMirror.EditorView(document.querySelector("#editor"), {
  //     state: ProseMirror.EditorState.create({
  //         schema: ProseMirror.basicSchema,
  //         plugins: plugins
  //     })
  // });
  </script>
```

js中有问题的代码:

```js
Object.defineProperty(exports, '__esModule', { value: true });
//浏览器报错: Uncaught ReferenceError: exports is not defined
```

如果用下面的办法搞, 那么就会报新的错误

```html
<script> var exports = {}; </script>
```

因此, 原因是, 我没有把编译这一步做好, 需要彻底的编译这个example.

仔细又看了一遍rollup的配置文件: rollup.config.js

```js
module.exports = {
  input: "./src/index.js",
  output: {format: "cjs", file: "dist/index.js"}, //这里指定了cjs, 所以浏览器不能用.
  sourcemap: true,
  plugins: [require("rollup-plugin-buble")()],
  external(id) { return !/^[\.\/]/.test(id) }
}
```

改为umd, 再次编译. 然后依旧报错

```
[!] Error: You must supply options.name for UMD bundles
```

这个意思是要指定name再配置文件

```js
module.exports = {
  input: "./src/index.js",
  output: {format: "umd", file: "dist/index.js"},
  sourcemap: true,
  plugins: [require("rollup-plugin-buble")()],
  external(id) { return !/^[\.\/]/.test(id) },
  name: 'mckmck', //这里随便起个名字.
}
```

这样编辑OK了, 但是, 依旧无法看到结果. 原因是上面的html有问题, prosemirror找不到. 原因是之前我给他的名字是mckmck, 所以html应该这样:

```js
var plugins = mckmck.exampleSetup({
  schema: mckmck.basicSchema
});
var view = new mckmck.EditorView(document.querySelector("#editor"), {
  state: mckmck.EditorState.create({
    schema: mckmck.basicSchema,
    plugins: plugins
  })
});
```

在官网发帖, 等待答复:

maybe we need more detail to run the example for fresh people just like me. i have change rollup.config.js to this:

```
module.exports = {
  input: "./src/index.js",
  output: {format: "umd", file: "dist/index.js"},
  sourcemap: true,
  plugins: [require("rollup-plugin-buble")()],
  external(id) { return !/^[\.\/]/.test(id) },
  name: 'mckmck', //the object for html
}
```

and in html:

```
var plugins = mckmck.exampleSetup({
  schema: mckmck.basicSchema
});
var view = new mckmck.EditorView(document.querySelector("#editor"), {
  state: mckmck.EditorState.create({
    schema: mckmck.basicSchema,
    plugins: plugins
  })
});
```

and then i run then dist/index.html in chrome. i get error:

Uncaught TypeError: Cannot read property ‘smartQuotes’ of undefined

