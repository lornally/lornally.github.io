> 2019-02-23 我初步的阅读的codemirror1的源码, 但是, 没有解决一个问题,  没有仔细看光标的处理, 当我对dom进行处理的时候, 怎么保持光标的正确呢?

阅读目标:

1. 拿到每键处理dom.
2. 每键处理dom时如何保持光标的正确性.

阅读方法:

1. 直接看源代码
2. chrome开调试跟踪代码
3. git跟踪提交记录找到相应的提交

这里看上去第二种方法比较合理那么一大波问题袭来

1. 如何调试动态加载的js代码

   ```js
   在需要跟踪的js的头部或者尾部加这个注释
   //# sourceURL=随便起的名字.js
   然后再chrome里面就能看到nodomain里面出现了对应的名字的js文件
   ```

2. pause后逐步执行js时, chrome给我看的是这个:

   ```js
   //tab显示VM1415
   !!(
       (window.__REACT_DEVTOOLS_GLOBAL_HOOK__ && Object.keys(window.__REACT_DEVTOOLS_GLOBAL_HOOK__._renderers).length) || window.React
     )
   ```

   - 询问全知全能的stackoverflow, 然后发现是react devtools的影响. 卸载了, 依旧没用. 

   - 重启chrome试试.

   - Stack Overflow有另一个解决办法, 在页面中加上这个:

     ```html
     <script>
     window.__REACT_DEVTOOLS_GLOBAL_HOOK__.inject = function () {}
     </script>
     很遗憾, 这个方法并没有解决问题.
     ```

3. 然后上面这个问题被我绕过去了, 在elements里面, 找到event listen, 选择blocking, 这样事件都会被跟踪.

4. 跟踪的过程中发现select.js是关键, 作者在这里实现了光标的各种操作.

5. 实际的dom操作在文件尾部的highlight方法中实际执行.

6. select.js -> selectionrange方法  和 selectiontopnode方法中大量使用range操作光标.

   ```js
   var range = window.getselection().getrangeat(0) //拿到range
   var node = start ? range.startContainer : range.endContainer; //拿到node, 就是正在处理的文本
   var offset = start ? range.startOffset : range.endOffset; //拿到光标偏移的量
   ```

7. 关键代码

   ```
   editor
   1459 highlight
   1081 对highlight的调用
   832 keydown 事件的响应函数
   1169 highlightParens
   1216 highlightAtCursor 以及周围的调用
   1075 highlightAtCursor 函数
   1081 对于highlight的调用时最关键的调用, 同时它的上下文用select操作了光标.
   1579 highlight里面的foreach是核心处理代码.
   993 keyup是最佳断点位置.
   1637 处理了contianer, 就是外部包围的span.
   1655 移除了多余的内容.
   最终断点位置: 1637, 此时可以移除keyup的断点. 不过这个位置会错过1216周围的光标操作.
   
   select
   521 selectionTopNode
   
   ```

8. 需要注释掉的异常抛出, 这些异常如果不注释掉, 会严重影响调试, 除非设置为不跟踪的模式.

   ```
   editor
   1602 抛出异常需要注释掉, 不然会频繁打断调试.
   1608 抛出异常, 也需要注释掉. 不然也会打断调试.
   until
   16 抛出异常
   
   最终结论: 这些异常抛出其实也是可以不注释掉的.
   是因为勾选了pause on excptions(在source面板的右边执行按钮下面)
   ```

###### 代码分析

核心代码在editor

```js
// 1165行
// Take the token before the cursor. If it contains a character in
// '()[]{}', search for the matching paren/brace/bracket, and
// highlight them in green for a moment, or red if no proper match
// was found.
highlightParens: function(jump, fromKey) {
  var self = this, mark = this.options.markParen;
  if (typeof mark == "string") mark = [mark, mark];
  // give the relevant nodes a colour.
  function highlight(node, ok) {
    if (!node) return;
    if (!mark) {
      node.style.fontWeight = "bold";
      node.style.color = ok ? "#8F8" : "#F88";
    }
    else if (mark.call) mark(node, ok);
    else node.className += " " + mark[ok ? 0 : 1];
  }
  function unhighlight(node) {
    if (!node) return;
    if (mark && !mark.call)
      removeClass(removeClass(node, mark[0]), mark[1]);
    else if (self.options.unmarkParen)
      self.options.unmarkParen(node);
    else {
      node.style.fontWeight = "";
      node.style.color = "";
    }
  }
  if (!fromKey && self.highlighted) {
    unhighlight(self.highlighted[0]);
    unhighlight(self.highlighted[1]);
  }

  if (!window || !window.parent || !window.select) return;
  // Clear the event property.
  if (this.parenEvent) parent.clearTimeout(this.parenEvent);
  this.parenEvent = null;

  // Extract a 'paren' from a piece of text.
  function paren(node) {
    if (node.currentText) {
      var match = node.currentText.match(/^[\s\u00a0]*([\(\)\[\]{}])[\s\u00a0]*$/);
      return match && match[1];
    }
  }
  // Determine the direction a paren is facing.
  function forward(ch) {
    return /[\(\[\{]/.test(ch);
  }

  // 这里开始是核心代码

  //这一行处理的是光标
  var ch, cursor = select.selectionTopNode(this.container, true);

  //这一行处理了高亮, 就是span那种, 具体调用下面分析.
  if (!cursor || !this.highlightAtCursor()) return;

  //这里开始继续搞光标
  cursor = select.selectionTopNode(this.container, true);
  if (!(cursor && ((ch = paren(cursor)) || (cursor = cursor.nextSibling) && (ch = paren(cursor)))))
    return;
  // We only look for tokens with the same className.
  var className = cursor.className, dir = forward(ch), match = matching[ch];

  // Since parts of the document might not have been properly
  // highlighted, and it is hard to know in advance which part we
  // have to scan, we just try, and when we find dirty nodes we
  // abort, parse them, and re-try.
  function tryFindMatch() {
    var stack = [], ch, ok = true;
    for (var runner = cursor; runner; runner = dir ? runner.nextSibling : runner.previousSibling) {
      if (runner.className == className && isSpan(runner) && (ch = paren(runner))) {
        if (forward(ch) == dir)
          stack.push(ch);
        else if (!stack.length)
          ok = false;
        else if (stack.pop() != matching[ch])
          ok = false;
        if (!stack.length) break;
      }
      else if (runner.dirty || !isSpan(runner) && !isBR(runner)) {
        return {node: runner, status: "dirty"};
      }
    }
    return {node: runner, status: runner && ok};
  }

  while (true) {
    var found = tryFindMatch();
    if (found.status == "dirty") {
      this.highlight(found.node, endOfLine(found.node));
      // Needed because in some corner cases a highlight does not
      // reach a node.
      found.node.dirty = false;
      continue;
    }
    else {
      highlight(cursor, found.status);
      highlight(found.node, found.status);
      if (fromKey)
        parent.setTimeout(function() {unhighlight(cursor); unhighlight(found.node);}, 500);
      else
        self.highlighted = [cursor, found.node];
      if (jump && found.node)
        select.focusAfterNode(found.node.previousSibling, this.container);
      break;
    }
  }
}

//1074
// highlightAtCursor是个比较简单的函数
// Re-highlight the selected part of the document.
highlightAtCursor: function() {

  //这里是之前的处理select的覅方, 所有的光标都在select
  var pos = select.selectionTopNode(this.container, true);
  var to = select.selectionTopNode(this.container, false);
  if (pos === false || to === false) return false;

  select.markSelection();
  //这里是实际处理span的地方
  if (this.highlight(pos, endOfLine(to, this.container), true, 20) === false)
    return false;
  //这里继续处理select. 所有的光标都在select
  select.selectMarked();
  return true;
}
//1528 这个parts就是移除元素所在的位置
// parts is an interface to make it possible to 'delay' fetching
// the next DOM node until we are completely done with the one
// before it. This is necessary because often the next node is
// not yet available when we want to proceed past the current
// one.
var parts = {
  current: null,
  // Fetch current node.
  get: function(){
    if (!this.current)
      this.current = traversal.nodes.shift();
    return this.current;
  },
  // Advance to the next part (do not fetch it yet).
  next: function(){
    this.current = null;
  },
  // Remove the current part from the DOM tree, and move to the
  // next.
  remove: function(){ //这里是移除
    container.removeChild(this.get());
    this.current = null;
  },
  // Advance to the next part that is not empty, discarding empty
  // parts.
  getNonEmpty: function(){
    var part = this.get();
    // Allow empty nodes when they are alone on a line, needed
    // for the FF cursor bug workaround (see select.js,
    // insertNewlineAtCursor).
    while (part && isSpan(part) && part.currentText == "") {
      // Leave empty nodes that are alone on a line alone in
      // Opera, since that browsers doesn't deal well with
      // having 2 BRs in a row.
      if (window.opera && surroundedByBRs(part)) {
        this.next();
        part = this.get();
      }
      else {
        var old = part;
        this.remove();
        part = this.get();
        // Adjust selection information, if any. See select.js for details.
        select.snapshotMove(old.firstChild, part && (part.firstChild || part), 0);
      }
    }

    return part;
  }
};

var lineDirty = false, prevLineDirty = true, lineNodes = 0;

//1575
//highlight 这个函数很复杂, 我们只拿核心出来分析
// This forEach loops over the tokens from the parsed stream, and
// at the same time uses the parts object to proceed through the
// corresponding DOM nodes.
forEach(parsed, function(token){
  var part = parts.getNonEmpty();

  if (token.value == "\n"){
    // The idea of the two streams actually staying synchronized
    // is such a long shot that we explicitly check.
    if (!isBR(part))
      throw "Parser out of sync. Expected BR.";

    if (part.dirty || !part.indentation) lineDirty = true;
    maybeTouch(from);
    from = part;

    // Every <br> gets a copy of the parser state and a lexical
    // context assigned to it. The first is used to be able to
    // later resume parsing from this point, the second is used
    // for indentation.
    part.parserFromHere = parsed.copy();
    part.indentation = token.indentation || alwaysZero;
    part.dirty = false;

    // If the target argument wasn't an integer, go at least
    // until that node.
    if (endTime == null && part == target) {
      console.log('StopIteration')
      return;
    }
    //fixm by m 为了调试注释这段, 替换为上面的大括号代码块. throw StopIteration;

    // A clean line with more than one node means we are done.
    // Throwing a StopIteration is the way to break out of a
    // MochiKit forEach loop.
    if ((endTime != null && time() >= endTime) || (!lineDirty && !prevLineDirty && lineNodes > 1 && !cleanLines)) {
      console.log('StopIteration')
      return;
    }
    //fixm by m 为了调试注释这段, 替换为上面的大括号代码块. throw StopIteration;
    prevLineDirty = lineDirty; lineDirty = false; lineNodes = 0;
    parts.next();
  }
  else {
    if (!isSpan(part))
      throw "Parser out of sync. Expected SPAN.";
    if (part.dirty)
      lineDirty = true;
    lineNodes++;

    // If the part matches the token, we can leave it alone.
    if (correctPart(token, part)){
      if (active && part.dirty) active(part, token, self);
      part.dirty = false;
      parts.next();
    }
    // Otherwise, we have to fix it.
    else {
      lineDirty = true;

      // Insert the correct part.
      //这里弄了一个新的span
      var newPart = tokenPart(token);
      //插入这个span
      container.insertBefore(newPart, part);
      if (active) active(newPart, token, self);
      var tokensize = token.value.length;
      var offset = 0;
      // Eat up parts until the text for this token has been
      // removed, adjusting the stored selection info (see
      // select.js) in the process.
      while (tokensize > 0) {
        part = parts.get();
        var partsize = part.currentText.length;
        select.snapshotReplaceNode(part.firstChild, newPart.firstChild, tokensize, offset);
        if (partsize > tokensize){
          shortenPart(part, tokensize);
          tokensize = 0;
        }
        else {
          tokensize -= partsize;
          offset += partsize;
          parts.remove(); //这里移除了原本的文本, 因为新的span格式好的插进去了.
        }
      }
    }
  }
};
        //tokenPart是一个比较简单的函数, 制造span, 其实不是.
        //1489
        // Create a part corresponding to a given token.
        function tokenPart(token){
  var part = makePartSpan(token.value);     
  part.className = token.style;
  return part;
};
//47
// Create a SPAN node with the expected properties for document part
// spans.
function makePartSpan(value) {
  var text = value;
  if (value.nodeType == 3) text = value.nodeValue;
  else value = document.createTextNode(text);

  var span = document.createElement("span");
  span.isPart = true;
  span.appendChild(value); //span里面放内容.
  span.currentText = text;
  return span;
}




```

光标代码在select

```js
//这个里面两套代码, 确实难受, 一套针对ie, 一套针对w3c, 在if里面做函数定义, 确实难受.

//219 是ie不需要看
//select.markSelection(); 有两个
//450
// Store start and end nodes, and offsets within these, and refer
// back to the selection object from those nodes, so that this
// object can be updated when the nodes are replaced before the
// selection is restored.
select.markSelection = function () {
  var selection = window.getSelection();
  if (!selection || selection.rangeCount == 0)
    return (currentSelection = null);
  var range = selection.getRangeAt(0);

  currentSelection = {
    start: innerNode(range.startContainer, range.startOffset),
    end: innerNode(range.endContainer, range.endOffset),
    changed: false
  };
};


//select.selectMarked(); 也有两个
//229 是ie不需要看

select.selectMarked = function() {
  if (!currentSelection || !currentSelection.changed) return;

  function makeRange(point) {
    var range = document.body.createTextRange(),
        node = point.node;
    if (!node) {
      range.moveToElementText(document.body);
      range.collapse(false);
    }
    else if (node.nodeType == 3) {
      range.moveToElementText(node.parentNode);
      var offset = point.offset;
      while (node.previousSibling) {
        node = node.previousSibling;
        offset += (node.innerText || "").length;
      }
      range.move("character", offset);
    }
    else {
      range.moveToElementText(node);
      range.collapse(true);
    }
    return range;
  }

  var start = makeRange(currentSelection.start), end = makeRange(currentSelection.end);
  start.setEndPoint("StartToEnd", end);
  start.select();
};


//467
select.selectMarked = function () {
  var cs = currentSelection;
  // on webkit-based browsers, it is apparently possible that the
  // selection gets reset even when a node that is not one of the
  // endpoints get messed with. the most common situation where
  // this occurs is when a selection is deleted or overwitten. we
  // check for that here.
  function focusIssue() {
    if (cs.start.node == cs.end.node && cs.start.offset == cs.end.offset) {
      var selection = window.getSelection();
      if (!selection || selection.rangeCount == 0) return true;
      var range = selection.getRangeAt(0), point = innerNode(range.startContainer, range.startOffset);
      return cs.start.node != point.node || cs.start.offset != point.offset;
    }
  }
  if (!cs || !(cs.changed || (webkit && focusIssue()))) return;
  var range = document.createRange();

  function setPoint(point, which) {
    if (point.node) {
      // Some magic to generalize the setting of the start and end
      // of a range.
      if (point.offset == 0)
        range["set" + which + "Before"](point.node);
      else
        range["set" + which](point.node, point.offset);
    }
    else {
      range.setStartAfter(document.body.lastChild || document.body);
    }
  }

  setPoint(cs.end, "End");
  setPoint(cs.start, "Start");
  selectRange(range);
};


// 521 
//select.selectionTopNode 一个很大的函数
```

###### 心得

- 格式化代码时延迟异步合并处理的.
  - 延迟是为了让他处理的时候, 内容是完整的. 因为, keydown时虽然我们拿到了code, 但是, 这个字还没有上到文本里面.
  - 合并是为了降低开销, 不影响用户输入.
- 单独维护一个文本内容. 不论什么方案都要维护文本内容. 然后记录是否dirty, 以行为线索记录. 
- 格式化的总的顺序
  - 采集光标
  - 格式化一个span, 加入文本内容, 插入
  - 删除原本的文本内容
  - 更新光标

###### 附录

- codemirror.js  676 lines
- editor.js  1674 lines
- highlight.js 70 lines
- mirrorframe.js 83 lines
- parsecss.js 163 lines
- parsejavascript.js 361 lines
- select.js 435 lines
- stringstream.js 160 lines
- Tokenize.js 58 lines
- tokenizejavascript.js 175 lines
- undo.js 414 lines
- Unittests.js 44 Lines
- Util.js 163 Lines