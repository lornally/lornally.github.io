> 这是好大的一个领域啊, 之前确实没关注.

###### 定义

Selection对象表示用户选择的文本范围或插入符号的当前位置。它代表页面中的文本选区，可能横跨多个元素。文本选区由用户拖拽鼠标经过文字而产生。要获取用于检查或修改的Selection对象，请调用 [`window.getSelection()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/getSelection)。

###### 相关使用

```js
text.selectionStart=10
text.selectionEnd=20
text.focus()  //这个写在这里也可以的. 
//这里text是任意的HTMLInputElement(感觉editable的元素应该都可以)
```

###### 概念

- **anchor**指向用户开始选择的地方，而**focus**指向用户结束选择的地方

  - 锚点（anchor）

    锚指的是一个选区的起始点（不同于HTML中的锚点链接，译者注）。当我们使用鼠标框选一个区域的时候，锚点就是我们鼠标按下瞬间的那个点。在用户拖动鼠标时，锚点是不会变的。

  - 焦点（focus）

    选区的焦点是该选区的终点，当您用鼠标框选一个选区的时候，焦点是你的鼠标松开瞬间所记录的那个点。随着用户拖动鼠标，焦点的位置会随着改变。

- [`ranges`](https://developer.mozilla.org/zh-CN/docs/Web/API/Range) 用户选择区域，俗称拖蓝。

- Selection.toString() 方法会返回被选中区域中的纯文本

```js
var selObj = window.getSelection();
var range  = selObj.getRangeAt(0);
```

###### 关键函数

[`focusNode`](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection/focusNode)

- 返回该选区终点所在的节点。

[`focusOffset`](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection/focusOffset)

- 返回一个数字，其表示的是选区终点在 [`focusNode`](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection/focusNode) 中的位置偏移量。

1. 如果 focusNode 是文字节点，那么选区末尾未被选中的第一个字，在该文字节点中是第几个字（从0开始计），就返回它。
2. 如果 focusNode 是一个元素，那么返回的就是在选区末尾之后第一个节点之前的同级节点总数。

[`rangeCount`](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection/rangeCount)

- 返回该选区所包含的连续范围的数量。

[`getRangeAt`](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection/getRangeAt)

- 返回选区开始的节点（[`Node`](https://developer.mozilla.org/zh-CN/docs/Web/API/Node)）。

[`collapse`](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection/collapse)

- 将当前的选区折叠为一个点。

###### range

- Range表示包含节点和部分文本节点的文档片段。

   `Range`可以用 [`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 对象的 [`createRange`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createRange)方法创建，也可以用[`Selection`](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection)对象的[`getRangeAt`](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection/getRangeAt)方法取得。另外，可以通过构造函数 [`Range()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Range/Range) 来获得一个 `Range `。

- [`Range.startContainer`](https://developer.mozilla.org/zh-CN/docs/Web/API/Range/startContainer) 只读

  返回包含 `Range `开始的节点。

- [`Range.startOffset`](https://developer.mozilla.org/zh-CN/docs/Web/API/Range/startOffset) 只读

  返回 startContainer 中表示 `Range `起始位置的数字。

- [Range.selectNodeContents()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Range/selectNodeContents)

  设定一个包含某个节点内容的 `Range` 。

- [`Range.insertNode()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Range/insertNode)

  在 `Range 的`起点处插入节点。

### 鼠标事件

###### 坐标

[`MouseEvent.clientX`](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/clientX) 只读

- 鼠标指针在点击元素（DOM）中的X坐标。

[`MouseEvent.offsetX`](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/offsetX) 只读

- 鼠标指针相对于目标节点内边位置的X坐标

[`MouseEvent.pageX`](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/pageX) 只读

- 鼠标指针相对于整个文档的X坐标；

[`MouseEvent.screenX`](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/screenX) 只读

- 鼠标指针相对于全局（屏幕）的X坐标；

[`MouseEvent.mozInputSource`](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/mozInputSource)  这个好, 要找到chrome对应的函数

生成事件的类型（若干 `MOZ_SOURCE_* `常量如下列出）。可通过该属性获知鼠标事件是否由真实鼠标设备触发，亦或通过触摸事件触发（这可能影响处理坐标事件时的精确程度

[`MouseEvent.x`](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/x) 只读

- [`MouseEvent.clientX`](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/clientX)的别名。

###### 模型

```
cancelled = !target.dispatchEvent(event)
```

- `event` 是要被派发的事件对象。
- `target` 被用来初始化 事件 和 决定将会触发 目标.
- 当该事件是可取消的(cancleable为true)并且至少一个该事件的 事件处理方法 调用了Event.preventDefault()，则返回值为`false`；否则返回`true`。

### UIEvent

[`UIEvent.layerX`](https://developer.mozilla.org/zh-CN/docs/Web/API/UIEvent/layerX)  只读

- 返回事件相对于当前层的水平坐标。
- pageX也是支持的.

### Event

三个方法加事件

- EventTarget.addEventListener
- 再html或者js xxx.onclick=function(event){}
- xxx.on("ooo", func) 这个写法并不是推荐写法.

[`Event.bubbles`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/bubbles) 只读

- 一个布尔值，用来表示该事件是否在DOM中冒泡。

[`Event.type`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/type) 只读

- 事件的类型（不区分大小写）

###### eventtarget

- EventTarget 是一个由可以接收事件的对象实现的接口，并且可以为它们创建侦听器。
- [`Element`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element)，[`document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 和 [`window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window) 是最常见的事件目标，但是其他对象也可以是事件目标，比如[`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)，[`AudioNode`](https://developer.mozilla.org/zh-CN/docs/Web/API/AudioNode)，[`AudioContext`](https://developer.mozilla.org/zh-CN/docs/Web/API/AudioContext) 等等。
- 许多事件目标（包括元素，文档和 window）还支持通过 on... 属性和属性设置[事件处理程序](https://developer.mozilla.org/zh-CN/docs/Web/Guide/DOM/Events/Event_handlers)。

两个target

- event.target标识事件发生的元素。
- event.currentTarget 当事件遍历DOM时，标识事件的当前目标。它总是引用事件处理程序附加到的元素