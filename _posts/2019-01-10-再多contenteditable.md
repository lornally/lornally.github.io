> https://hackernoon.com/easily-create-an-html-editor-with-designmode-and-contenteditable-7ed1c465d39b
>
> https://www-archive.mozilla.org/editor/midasdemo/

1. chrome的debug方式是从inspact element入手的. 可以一路跟踪过去.
2. 基本上操作都是依赖于document的.  这个editable的document貌似都能搞.
3. 用MutationObserver监视document的修改.

###### 事件

- DocumentOrShadowRoot.elementFromPoint()返回指定坐标处的最顶层元素。 实际上这个并不生效, 很神奇.

1. DocumentOrShadowRoot.elementsFromPoint() 返回指定坐标处的所有元素的数组, 这个也不生效

2. [`Document.createRange()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createRange)  新建一个

3. [`Document.createTextNode()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createTextNode) 新建一个文本节点(不含标签的节点).

4. [`DocumentOrShadowRoot.activeElement`](https://developer.mozilla.org/en-US/docs/Web/API/DocumentOrShadowRoot/activeElement) Read only

    返回得到焦点的element, 这个生效, 但是没啥用.

5. [`GlobalEventHandlers.onchange`](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onchange) 被改变.

6. [`Document.createElement()`](https://translate.googleusercontent.com/translate_c?depth=1&hl=en&rurl=translate.google.com&sl=en&sp=nmt4&tl=zh-CN&u=https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement&xid=25657,15700022,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh9h6UI45MhxgC0Bw-tUi0dZ8q3tw)

   使用给定的标记名称创建新元素

###### 貌似上面的都没用

真正有用的的是

- document.getselection
- Document.caretRangeFromPoint() 从光标位置拿到range.
- MutationObserver, 这个事件监控dom树的修改.
- execcommand应该有用, 看看是否有针对整行进行正则的做法.



> document也特别有用



```js
document.execCommand('insertHTML', false, '&#009') //在当前光标插入内容.
document.createTextNode('哈哈哈哈'); //建一个纯文字节点(不含标签)
document.caretRangeFromPoint(e.clientX, e.clientY); //拿到点, 可以插入内容的点.
```

> range特别有用

###### 拿到一个range

- `range`可以用 [`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 对象的 [`createRange`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createRange)方法创建，
- 也可以用[`Selection`](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection)对象的[`getRangeAt`](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection/getRangeAt)方法取得。
- 另外，可以通过构造函数 [`Range()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Range/Range) 来获得一个 `Range `。

```js
let range = document.caretRangeFromPoint(e.clientX, e.clientY);
range.insertNode(cur) // 这样就可以了, 之前是#cur的样式设置的不好.
range.insertNode(document.createTextNode("dadadadadada"));
let offset = range.startOffset;

```



> element特别有用单独形成一个blog吧. https://developer.mozilla.org/zh-CN/docs/Web/API/Element

###### 继承链

eventtarget <- node <- element <- htmlelement <- HTMLinputelement / HTMLimageelement......

###### 核心属性

- offsetWidth, 一个包含border和padding的宽度, 不含margin. 
- **getBoundingClientRect()**方法返回元素的大小及其相对于viewport的位置。 
  - 返回的是一个domrect(TextRectangle或 ClientRect 的别称), 
  - 包含top right bottom left

```js

```





