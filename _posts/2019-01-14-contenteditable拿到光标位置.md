###### 光标是两个单词

- caret 这个其实更准确
- cursor

###### 光标不是focus

dom有很多接口: https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model

###### 核心思路

1. 基于selection来做.
   2. window.getselection比较好. document.getselection也行.
   2. anchornode和anchoroffset可以得到位置.
   3. selection.iscollapsed可以用作判断, 判断的严谨一点. 这个可以描述是否选择状态, 还是仅仅是光标插入状态.
   4. document.execcommand可以做辅助功能.  他可以直接作用再selection上面, 可以认为是selection的快捷操作. 
2. range
   1. Document.caretRangeFromPoint() 可以拿到range的位置.
   2. selection.getrangat()
   3. range.endOffset
   4. Range.compareBoundaryPoints
   5. range.selectnodecontent 把整个node内容都选择了. 也就是说从range可以拿到包含他的node.
   6. range.startcontainer, endcontainer, commonancestorcontainer.
3. element接口继承自node
   1. document.elementfrompoint()可以拿到point(光标)指向的element. 一般情况都是个node.
   2. innerhtml和style以及attribute都在这个接口
   3. addeventlistener也在这里
   4. node.parentNode也在这里. 
   5. node.parentElement
   6. Node.textContent 拿到文本内容, 这个和value一样, 对于element会返回空, 不过他确实能拿到所有文本内容. 考虑用这个做文档的save.
4. caretposition是一个类似selection的接口.
   1. Document.caretPositionFromPoint(). 拿到element里面光标的具体位置.
5. window和document
   1. window负责scroll
   2. document负责createelement
6. mutationobserver dom变化的时候这个会被调用
7. 左边的行号, 记得设置一下contenteditable=false

###### css写法示例: 

```css
[contenteditable="true"].single-line {
    white-space: nowrap;
    width:200px;
    overflow: hidden;
} 
[contenteditable="true"].single-line br {
    display:none;

}
[contenteditable="true"].single-line * {
    display:inline;
    white-space:nowrap;
}
```

###### stackoverflow

###### 拿到光标

```js
sel = window.getSelection();
return sel.getRangeAt(0).startOffset;

//另一个写法
sel = window.getSelection();
range = sel.getRangeAt(0);
caretPos = range.endOffset;

var selObj = window.getSelection();
var selRange = selObj.getRangeAt(0);
cursorPos =  selObj.anchorOffset;
//这样可能也行
window.getSelection().anchorOffset

window.getSelection().getRangeAt(0).cloneRange();

//兜个圈子的做法
document.execCommand('insertHTML', false, '<span id="hidden"></span>');
var hiddenNode = document.getElementById('hidden');
var topPosition = hiddenNode.offsetTop;
hiddenNode.parentNode.removeChild(hiddenNode);


sel = document.getSelection(),
    nd = sel.anchorNode;


var range = win.getSelection().getRangeAt(0);
var preCaretRange = range.cloneRange();
preCaretRange.selectNodeContents(element);
preCaretRange.setEnd(range.endContainer, range.endOffset);
caretOffset = preCaretRange.toString().length;


range = document.createRange();//Create a range (a range is a like the selection but invisible)
range.selectNodeContents(contentEditableElement);//Select the entire contents of the element with the range
range.collapse(false);//collapse the range to the end point. false means collapse to end rather than the start
selection = window.getSelection();//get the selection object (allows you to change selection)
selection.removeAllRanges();//remove any selections already made
selection.addRange(range);//make the range you have just created the visible selection
```

###### 设置光标

```
var el = document.getElementById("editable");
var range = document.createRange();
var sel = window.getSelection();
range.setStart(el.childNodes[2], 5);
range.collapse(true);
sel.removeAllRanges();
sel.addRange(range);
```

###### 针对chrome的特别提醒

- Chrome does not show the caret in content-editable block elements without text content. 所以要用inline或者inline-block, 或者换成span.



###### 最后的总结:

- 两个关键api
  - range
  - selection
- 三个辅助接口
  - document
  - window
  - element