### observer

- https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver
- object.observer已经是被淘汰的接口, 目前的建议是: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy
- mutationobserver的处理优先级相当高, 早于正常的ui事件, 因此可以让整个处理更加流畅. Vue用了这个.

这里出现了一个小小的分支.

###### 如何查看dom树

- chrome的elements视图就是dom树的视图.
- https://developers.google.com/web/tools/chrome-devtools/
- element这边右键可以选择break on, 然后相关事件发生就会被break, 
  - 这个是发生在mutationobserver
  - 这个会显示在devtools->elements->dom breakpoints
- source那边也可以针对某个event进行breakpoints

###### 核心目标:

- 监控行的变化, 尤其是新增一行

###### 监控行变化的示例

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

 会触发两次, 分别是换行之前的内容, 和换行之后的内容.

> 这里有一个不确定, 未来如果我的编辑器包含了类似table这种东西, 那么我可能需要监控subtree的变动.

###### 抛弃未处理的变动

解决循环触发的问题, 有两个方面:

1. 只监控行变化, 不监控行内变化. childlist确实可以做到.
2. 抛弃未处理的变化, 

```js
observer.disconnect(); //可以再处理过程中用这个断开监控, 处理好了之后再调用: observer.observe();
observer.takeRecords(); //抛去本次处理队列中的剩余未处理事件.
```

###### 出现了一个重大疑问??

为啥div里面的回车会触发2次childlist变化, 而li里面的回车只触发一次呢?
我确认了, 一毛一样的代码, li只会触发一次div触发两次. pre也是触发两次. 不瞎bb, 看代码,  大家可以设置断点看看结果.: 

```html
<!DOCTYPE html>
<html>
    <head>
        <title>conntenteditable的div和list再mutationobserver监视之下的表现有重大差异</title>
        <style>

        </style>
    </head>

    <body>
        <div id='demo'>

            <ol contenteditable oninput="" style='border: 1px solid red'>
                <li>111111</li>
            </ol>

            <div id="ol" contenteditable oninput="" style='border: 1px solid red'>
                111111
            </div>
        </div>
    </body>

    <script type="text/javascript">
        var MutationObserver = window.MutationObserver || window.WebKitMutationObserver || window.MozMutationObserver;
        var list = document.querySelector('ol');
        var div = document.querySelector('#ol');

        var Observer = new MutationObserver(
            function(mutations, instance) {
                mutations.forEach(function(mutation) {
                    console.log(mutation);
                    if (mutation.type === 'childList') {
                        var list_values = [].slice.call(list.children).map(function(node) {
                            return node.innerHTML;
                        }).filter(function(s) {
                            if (s === '<br>') {
                                return false;
                            } else {
                                return true;
                            }
                        });
                        console.log(list_values);
                    }
                });
            });


        Observer.observe(list, { //监听list
            childList: true
        });

        Observer.observe(div, {  //监听div
            childList: true
        });
    </script>
</html>
```

从console.log也可以看到, li中的回车就一条数据, div的回车有一大堆数据.

###### 猜测

- 是否因为li和doc是不同的dom结构的问题?
- 可能这个猜测是对的, 直接在div里面添加div, mutationlist的长度就和li一样了. 也是1个元素了.

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>这个文档是在div里面添加div, mutationlist就只有一个了.</title>
        <style>
            #container {
                width: 300px;
                height: 300px;
                background-color: aquamarine;
                display: flex;
                align-items: center;
                justify-content: center;
            }
            .child {
                width: 100px;
                height: 100px;
                background-color: coral;
                font-size: 26px;
            }
            button {
                width: 300px;
                height: 40px;
                text-align: center;
                border: none;
                background-color: bisque;
                outline: none;
            }
        </style>
    </head>
    <body>
        <div id="container">
            <div class="child"></div>
        </div>
        <button id="update">
            Update
        </button>
        <script>
            var MutationObserver = window.MutationObserver || window.WebKitMutationObserver || window.MozMutationObserver
            const container = document.getElementById('container')
            const button = document.getElementById('update')
            const options = {
                childList: true, // 表示当前元素的childNodes发生变化的时候才会触发回调函数
            }
            // 创建MutationObserver实例，返回一个观察者对象
            const mutation = new MutationObserver(function(mutationRecoards, observer) {
                console.log(mutationRecoards)
                console.log(observer)
            })
            // 对观察者添加需要观察的元素，并设置需要观察元素的哪些方面
            mutation.observe(container, options);

            // 对container进行操作
            button.addEventListener('click', function() {
                container.appendChild(document.createElement('div'))
            })
        </script>
    </body>
</html>
```

###### 好遗憾

mutationobserver是个好工具, 但是对我而言几乎毫无用处, 因为, 即便用了mutationobserver, 我依旧无法摆脱keydown, 而如果我用keydown处理了文档, 那么mutationobserver就是毫无用处的. 

还有四份mutationobserver的文档没看, 列在这里: 

- https://juejin.im/post/5aee720df265da0b8f627173
  - 这篇文档还介绍了mutationobserver的几个竞争方案, 例如css动画
  - 他还介绍了以mutationobserver为基础的sessionstack: https://www.sessionstack.com/?utm_source=medium&utm_medium=blog&utm_content=mutation-observer-post
- https://segmentfault.com/a/1190000011072805
  - 这篇文当以vue源码的思路来介绍mutationobserver
- https://github.com/Ma63d/vue-analysis/issues/6
  - 同样是vue
- https://segmentfault.com/a/1190000017832686
  - 这是一系列的文章. 
  - 翻译自sessionstack, 嘿嘿, 就是第一篇文章的出处.