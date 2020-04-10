#### 传统事件
- onkeypress根本不会被中文输入法触发. 
  - 并且onkeypress的时候, 文字还没有真正的上去. node.nodevalue还没有真正改变.
  - keypress和keydown触发的时候content确实都还没有改变.
- oninput会自动把中文输入的第一个字符当做英文上来. 这个表现和后面的mutationobserver一样.
- onkeyup也不行, 问题和oninput一样.

###### mutation observer

```js
// 将要被监控的node
var targetNode = document.getElementById('some-id');

// 监视的事件列表
var config = {characterData: true, attributes: true, childList: true, subtree: true };

// 回调函数
var callback = function(mutationsList, observer) {
    for(var mutation of mutationsList) {
        if (mutation.type == 'childList') {
            console.log('A child node has been added or removed.');
        }
        else if (mutation.type == 'attributes') {
            console.log('The ' + mutation.attributeName + ' attribute was modified.');
        }
    }
};

// 一个事件监控的实例
var observer = new MutationObserver(callback);

// 开始监控, 针对哪个元素的哪些事件.
observer.observe(targetNode, config);

// 停止监控.
observer.disconnect();
```

###### 明白了 mutation observer 是一种简化

type等等都有极大的简化. 文档也变少了. 

###### mutation observer博大精深

1. 上面样例中的停止监控. observer.disconnect(), 一般情况下要注释掉, 除非你真的需要停止监控.
2. 监控的是innerhtml变化还是回车引起的换行变化, 这是两个写法. https://stackoverflow.com/questions/40195514/mutation-observer-not-detecting-text-change#

```js
// catch innerhtml 只要内部有变化就会被catch. 但是不会响应回车时间.
var config = { characterData: true, attributes: false, childList: false, subtree: true };
// catch textcontent  只有回车换行会被catch.
var config = { characterData: false, attributes: false, childList: true, subtree: false };
//这个写法就能catch 正常的输入和回车.
var config = {
      characterData: true,
      attributes: false,
      childList: true,
      subtree: true
    };
```

###### 那么问题来了 config的枚举型有几个选项? 只有四个吗?

```js
var config = {
    attributes: true,
    childList: true,
    subtree: true,
    characterData: true

};
```

不是的, 其实是葫芦娃七兄弟: https://developer.mozilla.org/en-US/docs/Web/API/MutationObserverInit

- 文档藏得太深了. 
- At a minimum, one of `childList`, `attributes`, and/or `characterData` must be `true`when you call [`observe()`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver/observe). Otherwise, a `TypeError` exception will be thrown. 这话的意思是childlist, attributes, characterdata必须有一个在监控列表, 不然就会抛出typeerror异常.
- MutationObserverInit 是个字典, 7个葫芦娃都定义再这里.
- 参考这份文档: https://davidwalsh.name/mutationobserver-api



###### mutationobserver何时触发?

To queue a mutation observer compound microtask, run these steps:

1. If mutation observer compound microtask queued flag is set, terminate these steps.
2. Set mutation observer compound microtask queued flag.
3. Queue a compound microtask to notify mutation observers.

参考: https://stackoverflow.com/questions/14564617/when-are-mutationobserver-callbacks-fired



