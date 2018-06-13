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





