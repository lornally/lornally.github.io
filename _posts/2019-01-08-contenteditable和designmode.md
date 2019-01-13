> contenteditable和designmode竟然还是不一样的.

- designMode只能把document整体改成可编辑状态，
- contentEditable可以把任何HTML元素改成可编辑状态
- 确定元素是否可编辑的唯一完全可靠的方法是检查 isContentEditable 属性
- document.getElementById("edit_area").contentEditable = true 

> 原文: http://qupyqupy.pixnet.net/blog/post/207305728-%E7%94%A8-contenteditable-%E8%88%87-designmode-%E5%89%B5%E9%80%A0%E8%87%AA%E5%B7%B1%E7%9A%84-html-%E7%B7%A8

###### 核心样例代码: 

html

```html
<div contenteditable="true"></div>
```



js

```js
document.execCommand("fontsize", false, 60);//这个document就是被选择的edatable元素.
document.getElementById("edit_area").contentEditable = true ; // true || false 
```



> 原文: https://juejin.im/entry/5b1f2989e51d4506d936fbd2
> 真正的原文: https://denzel.netlify.com/js/editable_dom.html?_=54325454321234

###### isContentEditable

- 一个只读的属性，返回true或false，表示页面元素是否可编辑。
- document.documentElement.isContentEditable   //true
- document.body.isContentEditable   //true
- document.querySelector('#id').isContentEditable   //true

###### designMode

- 是document的一个全局开关，可读可写，有两个值：“on”、“off”，表示整个页面是否可编辑。可以在console里输入: 
- document.designMode = 'on'

###### user-modify

- 是CSS版的contenteditable。
- user-modify: read-only;   //只读
- user-modify: read-write;  //读写，支持富文本
- user-modify: write-only;  //
- user-modify: read-write-plaintext-only;  //读写，但只可输入文本

###### contenteditable

- contenteditable=""  //与contenteditable="true"效果一样
- contenteditable="events"
- contenteditable="caret"
- contenteditable="typing"
- contenteditable="plaintext-only"  //可编辑，只允许纯文本
- contenteditable="true"			//可编辑，支持富文本
- contenteditable="false"     //不可编辑

###### js

当元素为editable状态时

- document.execCommand生效, 作用于活跃(被选择的)的部分. 
- 元素原本注册的时间就都失效了.

参考: https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Editable_content

###### document.execCommand

- 参考: https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand#Commands

貌似关键点在这里. 神奇了......

- formatBlock貌似可以用来处理当前行.  其实不行