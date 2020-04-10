###### 20180614  event补充

参考: https://www.kirupa.com/html5/javascript_events.htm



```js
//目前的王道: 
source.addEventListener(eventName, eventHandler, false);
```

enentName一共也没有多少:

- click  
- mousedown
- mouseup
- mouseover mouseout
- mousemove
- mouseenter  mouseleave 没有冒泡事件
- dblclick 尽量别用的click同时起作用.
- contextmenu
- mousewheel and DOMMouseScroll
- 
- DomContentLoaded
- load
- keydown
- keypress
- keyup
- scroll
- weal & DOMMouseScroll

基础概念

- capture 是第三个参数
  - true是在捕获(下沉)阶段生效
  - false是在冒泡阶段生效
- 打断事件链:  e.stopPropagation()
- 如果要阻止html默认动作: preventDefault

什么情况下, 需要考虑下沉和冒泡, 大神指出了这5个情况

1. Dragging an element around the screen and ensuring the drag still happens even if my mouse cursor slips out from under the cursor
2. Nested menus that reveal sub-menus when you hover over them
3. You have multiple event handlers on both phases, and you want to focus only on the capturing or bubbling phase event handlers exclusively
4. A third party component/control library has its own eventing logic and you want to circumvent it for your own custom behavior
5. You want to override some built-in/default browser behavior such as when you click on the scrollbar or give focus to a text field

鼠标事件

```js
document.addEventListener("contextmenu", hideMenu, false);
//组织默认的鼠标右键事件
function hideMenu(e) {
    e.preventDefault();
}
```

- screenX 屏幕上的位置.
- clientX 浏览器里面的位置.
- e.button = 0 左键, =1 中键, =2 右键
- mousewheel 滚轮事件 e.wheelDelta || -1 * e.detail 定义了滚轮方向.

键盘事件

- keycode 返回的是键值
- charcode  只响应keypress, 返回的是ascii码
- metakey ctrlkey altkey shiftkey

```js
window.addEventListener("keydown", keysPressed, false);
window.addEventListener("keyup", keysReleased, false);
 
var keys = [];
 
function keysPressed(e) {
    // store an entry for every key pressed
    keys[e.keyCode] = true;   
    // Ctrl + Shift + 5
    if (keys[17] && keys[16] && keys[53]) {
        // do something
    }     
    // Ctrl + f
    if (keys[17] && keys[70]) {
        // do something     
        // prevent default browser behavior
        e.preventDefault(); 
    }
}
function keysReleased(e) {
    // mark keys that were released
    keys[e.keyCode] = false;
}
```

模拟事件

```js
cb.dispatchEvent(document.createEvent("MouseEvents").initMouseEvent("click", true, true, window,
    0, 0, 0, 0, 0, false, false, false, false, 0, null));
//实际上不需要那么麻烦.
cb.click() //这样就可以了. 应该也是可以把event参数设置好的.
```





###### 20180112

> 今天在阅读monaco和codemirror时, 发现自己对于js事件处理的理解不到位. 补一下

### dom

- 这个确实需要先看: https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model/Introduction
- dom都有那些接口: https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model
- 更基础的: https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs

### element->document

一个具备document接口的对象, 这个特别有用, 我们从html拿到的都是element.https://developer.mozilla.org/zh-CN/docs/Web/API/Element

### window -> eventTarget

也从eventTarget继承了属性, https://developer.mozilla.org/zh-CN/docs/Web/API/Window

### document->node->eventTarget

document接口继承了node接口, 内容巨多: https://developer.mozilla.org/zh-CN/docs/Web/API/document

### node->eventTarget

- node 是一个接口
- 他从eventTarget继承,
- 定义了一堆我们经常用的函数, 太多了, 直接查api吧: https://developer.mozilla.org/zh-CN/docs/Web/API/Node





### eventTarget

eventtarget接口和事件的关系就是核心关系了

```
EventTarget.addEventListener()
在EventTarget上注册特定事件类型的事件处理程序。
EventTarget.removeEventListener()
EventTarget中删除事件侦听器。
EventTarget.dispatchEvent()
将事件分派到此EventTarget。
```

- https://developer.mozilla.org/zh-CN/docs/Web/Events
- https://developer.mozilla.org/zh-CN/docs/Web/API/Event
- https://developer.mozilla.org/zh-CN/docs/Web/Guide/Events
- https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks/Events

```js
xxx.on("ooo", func)
xxx.onOOO(func)

标准的做法: myButton.addEventListener('click', function(){
ie的做法: element.attachEvent

myButton.onclick = function(event){alert('Hello world');};



var divs = document.querySelectorAll('div');

for (var i = 0; i < divs.length; i++) {
  divs[i].onclick = function(e) {
    e.target.style.backgroundColor = bgChange();
    //e.target的意思是发生事件的element
  }
}
  
//事件是冒泡传递的. 但是, 我们可以阻止它.
video.onclick = function(e) {
  e.stopPropagation();// 这个阻止了事件冒泡
  video.play();
};
```





