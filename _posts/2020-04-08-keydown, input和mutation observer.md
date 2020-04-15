> 这三者的情况, 之前都试验过. 是时候做个总结了.

事实上2019年1月-3月我连续发了6篇关于这三个玩意的blog.

###### onkeyup和onkeydown的区别

- keydown 最先被触发, 需要主力使用的事件.
- keypress 不会被中文触发. 输入了文字, 但是, 文字还没有上去. 被废除, 并且很多按键不能触发.
- keyup 最后触发, 文字上去了已经了, 因此不适用于大部分情况, 快捷键可能需要他. 

keydown有一个问题要注意, 某些时候keydown不能正确执行, 比如:

```js
editor.onkeydown=editinput;
function editinput(e) {
  if(e.keyCode==32) {//space
    var range=window.getSelection().getRangeAt(0);
    var node=range.createContextualFragment('ttttttttttt');
    range.insertNode(node); //这个不会被执行, 除非设置断点用devtools跟踪.
  }
}
```

设置了timeout之后就OK了

```js
if(e.keyCode==32) {//space
    setTimeout(dealpace,0);
    e.stopPropagation(); 
    e.preventDefault();  
}
function dealpace() {
    var range=window.getSelection().getRangeAt(0);
    var node=range.createContextualFragment('ttttttttttt');
    range.insertNode(node);
}
```

###### input

input

- editable没有change事件, 只有input事件, 确切的说是change事件不一定每次都被触发.
- input的触发没有问题, 但是: 没有keycode, key, charcode, code这些. 

###### mutation observer 可以干啥?

- 处理空格可以吗?
- 处理undo/redo有优势吗?

## 附加信息

###### 两个target

- event.target 事件发生的元素。
- event.currentTarget 事件处理程序写在哪个元素上.

###### 三个方法加事件

- EventTarget.addEventListener
- 在html或者js xxx.onclick=function(event){}
- xxx.on("ooo", func) 不推荐

###### 曾经写过的blog

- 2019-03-19 mutation observer
- 2019-02-28 事件

###### observer 示例

```js
var config = {
    childList: true, //这一行是关键, 这个事件对应换行操作.
};
var callback = function(mutationsList, observer) {
    for (var mutation of mutationsList) {
        console.log('A::', mutation.type);
    }
};
var observer = new MutationObserver(callback);
observer.observe(diveditor, config);
```

###### 事件模型

微任务排在宏任务之前, 

1. micro包括
   1. promise.then
   2. mutationobserver. 
   3. process.nextTick 优先级高于Promise, 这是一个node概念.
2. macro包括
   1. timeout
   2. i/o
   3. 以及交互事件

###### shadow dom

- dom本身到了element就是原子了, shadow dom可以描述一个element.
- 举个例子就是video元素, 再里面有各种按钮和操作, 这都是shadow dom发挥作用的地方.
- shadow dom用一棵树来描述一个element.
- 这个element自身包含了code, css, js….也就是说, 形成了一个html闭包.
- shadow dom的本质是封装.
- shadow dom是custom elements的一部分.

###### web components

自定义标签终于形成了完整的规范, 它包含三部分:

- custom element, 一组js api, 可以自定义标签以及他的hebaviour, 
- shadow dom, 一组js api, 可以定义一个元素的内部结构.
- html templates, html的模板标签, 可以在上面两个api里面重复使用.

###### element

继承链: eventtarget <- node <- element <- htmlelement <- HTMLinputelement / HTMLimageelement.

###### todo 待了解内容

- mutation observers
  - 这个需要缓存选择 preserving selection
  - 或许还需要处理undo manager
- clipboard api