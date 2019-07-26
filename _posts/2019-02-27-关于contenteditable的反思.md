> 虽然之前看了不少内容, 但是, 最近艰难的再这个领域跋涉, 遇到很多问题, 进度太过喜人, 因此, 我们值得再次反思一下.

### 第一部分 editable的善与恶

###### 参考mdn

- https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Editable_content
- 这里还提到了midas
- caret-color

- https://ckeditor.com/blog/ContentEditable-The-Good-the-Bad-and-the-Ugly/
- 本文章第一部分, 就围绕这个主题来写, 主要是上面这份文档的思路. 

###### 现状

- 市场上依旧没有统治级的编辑器. 没有好评爆表的编辑器. 那么, 我们做一个怎么样?
- 我们手上的材料:
  - contenteditable
    - 回车直接添加div太恶心了. 
    - 粘贴时自动style也太恶心了.
  - execCommand
    - selection, 如果这个考虑方向就更复杂了.
    - ranges
  - queryCommandState
  - mutation observers
    - 这个需要缓存选择 preserving selection
    - 或许还需要处理undo manager
  - 为了对抗inline style, 你不仅仅要处理粘贴, 回车…...
    - backspace
    - delete

###### 由此我们很容易得出结论

- contenteditable是魔鬼
- selection也是魔鬼
- 所以尽量少用他们
  - 自己建一套selection机制, 类似codemirror2/5, vscode这种.
  - 监听用户的输入, 监听键盘事件(其实需要一个input才行, 不然很难对付输入法)
  - 你要处理光标运动, 各种上下左右以及组合键和快捷键.
  - 你要自己处理粘贴, clipboard api要去研究下.
- 然后, 可以使用keyboardevent.key来处理用户输入.
- 可以使用composition event来处理输入法输入.
- 同时, 作者在这里也指出了, 用一个隐藏的textarea来接受用户输入. 并且把这个input放到隐藏的光标caret下面.
- w3c也有一个opening the ime api, 正在制作中.
- 中文如何分词? 以响应option+移动/删除

移动设备的问题出现了:

1. 键盘怎么弹出来? 隐藏textarea应该可以.
2. paste怎么做?
3. 摇动手机做undo? accelerometer
4. 再手机上做一次选择, 键盘不见了.
5. 拼写检查??
6. 读屏软件无法阅读你的编辑器, 用户的疑问是, 你是否阅读过w3的最佳实践文档.

> 忍不住说一句, 这个作者太厉害了, 2015年他写了这篇文章, 如果我早看到, 就可以少走很多弯路. 他的思路历程和codemirror的作者一毛一样.

###### 然后怎么办? 路在何方?

- w3c成立了编辑器讨论组, Frederico Knabben, piotrek koszulinski, 都在里面
  - https://lists.w3.org/Archives/Public/public-editing-tf/
  - http://w3c.github.io/editing/
  - https://www.w3.org/wiki/Editing_Taskforce
- 作者的编辑器是: CKEditor, medium.com(不是, medium编辑器和medium没关系)
- 如果使用editable, 我们需要实现:
  - 回车
  - 各种类似execcommand的指令
  - undo manager
  - 还要打断插入切面做pasted, 以便做filter.
  - 选择api也要重写
  - 列表和table也要自己实现.
  - 选择, 键盘, 焦点, 剪贴板要一体的重写.
  - 删除也要控制, backspace/delete.
  - copy, cut, paste要全权控制.
- 这其实意味着, 编辑器不要求浏览器做任何事, 除了帮助handle用户输入之外, 任何事都不依赖浏览器以及contenteditable.
- 作者也发现DOM作为基础结构也很有问题.

最后附上作者大名: Piotr Koszuliński   CKEditor 5 Lead Developer





###### 参考编辑器

- <https://github.com/atom/xray>
- https://github.com/xi-editor/xi-editor
- 这两个都是rust写的: https://en.wikipedia.org/wiki/Rust_(programming_language)

### 第二部分 w3c的对策

- 主要参考: http://w3c.github.io/editing/
- 编辑器组: <https://lists.w3.org/Archives/Public/public-editing-tf/>
- 编辑器组的入口: https://www.w3.org/wiki/Editing_Taskforce

w3c成立这么一个组织. 用来改进editable. 这个组织里面都是领域的大神.  大神们的改进策略:

- editable只是用来采集用户输入, 其他附加的事少干. 

目前的问题:

1. editable太过复杂, 并且充满bug.  准备通过规定好表现来解决. 
2. editable不容易适应编辑场景(剧本)的多样化. 专门有个列表展示现在不支持的编辑方式.

### 第三部分 w3c的规范

http://w3c.github.io/editing/contentEditable.html

这个要有空再看了. 而且确实需要细看



### 第四部分 原作者的另一个blog

- https://medium.com/content-uneditable/fixing-contenteditable-1a9a5073c35d

- 原文非常通俗易懂,  我就不翻译了.

列一点核心观点:

1. 关键字: contentEditable=’minimal’
2. selection api很关键, 并且需要完善.
3. contentEditable=’minimal’进化了一群小弟: *contentEditable=typing*, *contentEditable=cursor* , *contentEditable=events* 
4. 特别介绍了这个: *ContentEditable=events* 这个模式下, 浏览器不会改变dom.



### 第五部分  editable的罪与罚

1. 我的读书笔记: 2019-01-09-medium为什么editable是泰瑞宝, 
2. 原文: https://medium.engineering/why-contenteditable-is-terrible-122d8a40e480
3. 作者声明了两件事:  很神奇, 再次阅读时没有下面两点了, 并且原文中的插图也都不见了. 我陷入了深深的思索.
   1. medium-editor和medium没关系.
   2. 他推崇的是prose-mirror.
4. 作者是chrome的作者之一.

> 最后再顺便吐槽一下, 为毛中文就没有有用的信息呢? 

### 附录 各个editable编辑器的实现方式

1. Codemirror1 用的iframe designmode/contenteditable
2. Codemirror6 用的pre  contenteditable
3. ckeditor 4 用的iframe->body contenteditable
4. ckeditor 5直接用div了 contenteditable
5. tinymce用的iframe->body contenteditable
6. prosemirror 用的div, 回车处理为p + br

