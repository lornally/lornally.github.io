> 接说上文, 这一片主要跟随作者的blog: codemirror2究竟是咋回事: https://codemirror.net/doc/internals.html

### 前言

- 作者明言: 这一片是之前那一片code1咋回事的后续.
- 这个code2会比较简明, 不像code1那么绕. 

###### code1的问题是啥?

- designmode/editable的浏览器支持有各种问题. 不仅仅是ie, webkit和firefox都有各自的问题, 不过ie还是很顽固, ie的问题在于, 一旦他有问题, 你就改不动. 
- frame是另一问题, 回退和初始化以及跨域都会有各自的问题.
- 选择selection也出现了问题 ie的做法与众不同的愚蠢, opera也是刚刚才修好了问题, codemirror1里面有700行代码处理selection.
- 由此导致了, 系统中的辅助性(兼容性)代码比例太高了, 高到了一个可怕的量.  每次浏览器更新都可能引起灾难性的后果.

###### 2 想干啥?

- 最主要的就是避开1面临的问题, 这里作者感谢ace给出的解决方案. (这个也是我想要的解决方案).
- 但是, marijn并不想完全控制用户的输入. 不仅仅是key event本身就有各种问题, 还在于根本没办法处理类似东亚字符集这样的输入.

###### 所以

- 所以marijn构建了一个隐藏的textarea获得focus, 并且让浏览器认为用户正在这个textarea里面输入.
- 然后我们展示给用户的是一个dom, 从用户的输入生成的dom.
- 只要再把光标(弄一个假的)显示正确, 那么看上去这个编辑器就很正常了.

###### 附加的好处

- 不必再处理整个文档了. (只要处理最近输入的就行.)
- editable的dom本身比正常dom要慢.
- 剩下的就是处理onscroll了.

### 输入

- ace只是使用textarea输入文字, 光标移动的删除, 都是直接用key event处理. 
- codemirror期望让浏览器尽量多的处理. 

###### 有一个思路

- 有一个思路, 就是把整个文档到放到这个textarea里面. 然后每个key event都更新对应的dom.
- 这样做会比较简单(这个我有深深的疑问, 恐怕不那么简单).
- marijn认为这样会比较慢.

###### cm2的思路

- textarea里面保留当前行的前一行和后一行.
- 这个有道理, 这样确实不用处理一般情况的光标移动和删除了. 
- 而且无论如何也要随时更新的, 这样的更新范围还是比较合理的.
- marijn只需要读取textarea里面的cursor, 然后更新外边的cursor就好了.
- copy和paste也直接按照系统的情况来…….(这个我做的时候要想一下, 是否需要把dom->md->dom, 当然dom->dom或许也可以).
- 如果用户操作跨越了textarea的范围, 例如page up/down, ctrl - end, 或者鼠标点击, 这些事件都要单独处理了. 

###### 选择selection

- textarea里面的selection很好处理, 用selectionstart和selectionend就可以了.
- 此处有了疑问, 就是说选择内容都维护在textarea里面? 用鼠标选择的内容咋办? 估计也维护到textarea里面.
- 最终marijn还是用key event来处理这个. 并且记录用户移动光标所用的key组合.
- 我估计鼠标选择也要基于key event, 比如shift和cmd
  - 点选需要shift
  - 拖选恐怕就是拖动事件了

###### 更新

