> 要写编辑器, codemirror是无法逾越的大山, 以以一己之力对抗atom和vscode. 并且是硕果仅存的js古代编辑器, 如果不是被google code坑了, 这货应当是三分天下的地位.

###### 缘起

想弄一个分布式知识库, 然后发现markdown解析绕不开, 然后到网上找项目, 怎们绕最后都绕道了codemirror或者prosemirror. 这两个都是大神marijn haverbeke的作品, 大神的另一个作品就是名闻遐迩的eloquent javascript. 

######  codemirror分两段

1. Codermirror1 基于designmode和editable构建的. 大神被浏览器各种虐, 尤其是ie.
2. Codemirror2 之后基于一个hidden的textarea来实现, 这个textarea隐藏在mockup的光标下面. 这个思路很巧妙.

多年之后, 浏览器不那么虐了, 大神又用codemirror1的思路实现了prosemirror.

###### 阅读之源

1. 大神官网: http://marijnhaverbeke.nl/
2. 大神的blog一定要看: http://marijnhaverbeke.nl/blog/
3. codemirror官网: https://codemirror.net
4. 官方的git: https://github.com/codemirror

官网的文章如果都看了那么就差不多了. 

###### 阅读之路

1. 介绍codemirror1是咋回事, https://codemirror.net/1/story.html
2. 介绍codemirror2是咋回事, https://codemirror.net/doc/internals.html

###### codemirror1

> 作者再文中讲述了自己的艰辛. javascript做啥都很难.

1. 第一步作者只想安安静静的在textarea中实现缩进.
   1. 即便这样, 自动缩进的时候, 不要让光标乱跳, 已经是令人头疼的事情了.
   2. w3c规定了selectionstart和selectionend, 这个很棒. 但是, ie不支持啊. 所以针对ie要另写textrange.
   3. 然后使用textrange的时候, 回车并不被计入字符, 所以要针对有多少回车(行数)写兼容代码矫正.
   4. 陪ie玩够了之后, 再陪firefox玩, 然后发现彻底玩不动了. 这货在操作dom的text content时太慢了.
2. 然后作者找到了designmode/editable
   1. 然后, 作者指出ie在这个事情上的实现很糟糕, 更糟糕的是, 其他的浏览器在学他.
   2. 作者觉得自己在走歪路(后面codemirror2里面有更精确的描述)
3. parser, 为了做高亮, 作者竟然真的做了一个语法parser......
   1. 一个好的parser可以区分变量和属性.
   2. 甚至于还可以区分全局变量和局部变量, 作者发现自己for的写法错了. 话说这么干合适吗? 真的给js实现一个parser?
   3. 然后作者分析了, js不适合每次键入内容都parse全文. 因为js喜欢用闭包搞定名字空间作用域.
   4. 作者用了一个可打断, 可以断点续传的parse, 在每一行的结尾都记录parse状态. 作者太狠了, 看到这里, 我已经明白了, 我和作者要做的不是同一个东西. 我要做的东西对作者而言是小菜一碟.
4. 然后, 作者开始和dom玩命.
   1. 一开始作者想用white-space: pre 然后发现这货浏览器支持特别差, 尤其是editable的iframe.
   2. 然后作者想用div作为每一行.
   3. 最终作者找到了解决方案, 只用span和br, 这样整个就是扁平线性的, 完全没有嵌套. 
   4. 然后作者希望能处理html代码, 那么有类似之前parse的做法, 也就是不必每次都处理整个buffer, 只需处理有改动的地方, 并且作者还要优雅, 因此作者想到了iterator, 嘿嘿, 我当初也是这么写tailbase库的. 作者使用了mochikit库
   5. 高亮的做法步骤
      1. 整理dom树, 把他正常化(避免崩溃)
      2. 拿到文本
      3. 标注(token)这些文本
      4. parse这些标注(token)
      5. 调整标注对应的dom元素的显示样式
   6. 上面的12两步, 作者用了一个迭代生成器
      1. 这里作者贴入了代码.                          
      2. 如果不是span和br 那么处理一下.
      3. 如果是span, 拿内容. 搞token
      4. 如果是br, 替换为回车. 搞token
      5. 简单的说就是把dom变回正常文本, 然后作分析, 同时还要记录这些文本属于哪个dom元素.
      6. cc记录了当前的状态值. 
   7. 然后, 系统该考虑把文字和节点处理为token了. 他包含以下内容:
      1. value .其实就是文本.
      2. parser判定的元素类型, 比如: 变量, 操作符, 大括号......
      3. style, 未来给span的class用, 也就是显示类型, 合格未来还可能被parser调整.
      4. 正则表达式也可以再token这步解决, 因为他的开始字符只有有限的情况.
   8. 然后是parser这一步了. 
      1. 对于很多元素没有必要搞的太细, 比如if, else, while, try….这些都可以统一处理, else前面是否有if并不重要. 
      2. 但是, 对于var function这样的内容要细致一点, 因为要区分局部变量的作用域.
      3. parser存储了3种内容:
         1. 局部变量组, 每个函数都有一个局部变量组. 进入一个函数内部, 就压栈一组局部变量, 函数处理完了, 这一组就出栈.
         2. 上下文内容, 这个取决于我们是否在一个代码块里面. 这个块可能是各种括号, 引号构成.
         3. 持续内容组, 
      4. parser并没有使用线性的continue策略, 而是使用了堆栈的策略.
      5. 一块一块的去处理, 遇到封口, 就处理完当前块, 然后回到上一个层级的块.
   9. 最后处理高亮 , 依照两个流
      1. token流
      2. dom tree 
      3. 如果都正常, 这两块是能够匹配的. 找到合适的span进行修改或者插入span.
   10. 这个段落的最后, 作者还描述了br的处理, 处理js源码真心比markdown复杂太多了.
5. 选择
   1. 高亮处理之后, 必须要保证光标位置正确. 
   2. 每行处理, 因为每行末尾都存储了当前的parser状态.
   3. 因为每键处理会影响效率, 所以作者用了空闲300ms处理的机制, 自己维护一个dirty nodes列表.
   4. 每次最多处理十行dirty nodes.
   5. 一开始作者希望吧ie包装的好像满足w3c标准, 但是最终发现只要做两个操作:
      1. 把选择内容做个快照, 以便高亮之后, 再回复这个选择.
      2. 找到包含光标或者光标之前的node, 然后标记为dirty的.
   6. w3c的标准处理更难一点
      1. 用range能够拿到node, 也能拿到offset, 创建一个指向selectiton的start和end的对象
      2. 记录这些node关联的这个对象.
      3. 替换的时候, 看这样的引用是否存在, 再决定是否移动或者替换.
   7. 为了对付ie, 作者再次使用hack方法.
   8. opera怎们都不正确, 要等opera自己修复.
   9. 对于回车的处理ie依旧很诡异, 他直接用p标签包围. 所以作者只能自己拿这个key了.



这个笔记已经够长的了. 第二篇再写吧