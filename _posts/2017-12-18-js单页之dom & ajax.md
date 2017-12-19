### webapi

```
什么是API?

应用程序接口（API）是一些编程语言内结构，这些结构让开发人员更容易地构建复杂功能。它们把你和复杂的功能代码隔离开，让你能轻松简便的使用它们。

来看一个现实世界中的例子：想想您的房子、公寓或其他住宅的电力供应，如果您想在房子里使用一个电器，您只需把插头插入到一个插座中；您并不会直接把它连接到电源上 - 这样做效率会非常低，而且如果您不是电工，这么做既困难又危险。
```

### dom

- Element node: An element, as it exists in the DOM.
- Root node: The top node in the tree, which in the case of HTML is always the HTML node (other markup vocabularies like SVG and custom XML will have different root elements).
- Child node: A node directly inside another node. For example, IMG is a child of SECTION in the above example.
- Descendant node: A node anywhere inside another node. For example, IMG is a child of SECTION in the above example, and it is also a descendant. IMG is not a child of BODY, as it is two levels below it in the tree, but it is a descendant of BODY.
- Parent node: A node which has another node inside it. For example, BODY is the parent node of SECTION in the above example.
- Sibling nodes: Nodes that sit on the same level in the DOM tree. For example, IMG and P are siblings in the above example.
- Text node: A node containing a text string.

```javascript
document.querySelector()//拿到第一个满足条件的
document.querySelectorAll()//拿到所有满足条件的
var sect = document.querySelector('section');
var para = document.createElement('p');
para.textContent = 'We hope you enjoyed the ride.';
sect.appendChild(para);
var text = document.createTextNode(' — the premier source for web development knowledge.');
var linkPara = document.querySelector('p');
linkPara.appendChild(text);
sect.removeChild(linkPara);
linkPara.parentNode.removeChild(linkPara);
```

### 改变样式

```javascript
para.style.color = 'white'; //直接改属性
para.style.backgroundColor = 'black'; //css里面是连接符background-color
para.style.padding = '10px';
para.style.width = '250px';
para.style.textAlign = 'center';

para.setAttribute('class', 'highlight');// 直接附上一个css的class
```

### window

```javascript
var div = document.querySelector('div');
var WIDTH = window.innerWidth;
var HEIGHT = window.innerHeight;
div.style.width = WIDTH + 'px';
div.style.height = HEIGHT + 'px';
window.onresize = function() { //这个可以做到跟随窗口调整div的尺寸
  WIDTH = window.innerWidth;
  HEIGHT = window.innerHeight;
  div.style.width = WIDTH + 'px';
  div.style.height = HEIGHT + 'px';
}
```

### 参考

- https://developer.mozilla.org/en-US/docs/Web/API/Node
- https://developer.mozilla.org/en-US/docs/Web/API/Document

### ajax

- `XMLHttpRequest` or Fetch to request JSON

```javascript
var request = new XMLHttpRequest() //XHR request
request.open('GET', url)
request.responseType = 'text'
request.onload = function() {
  poemDisplay.textContent = request.response;
}
request.send()
```

- fetch这个是现代的写法

```javascript
fetch(url).then(function(response) {
  response.text().then(function(text) {
    poemDisplay.textContent = text;
  });
});//真丑陋, 括号套括号的写法. 有空看看是否能改成链式调用
fetch(url).then(function(response) { //链式写法
  return response.text()
}).then(function(text) {
  poemDisplay.textContent = text;
});

response.json()//拿到json
response.blob().then(
  function(blob) {//拿图片
    objectURL = URL.createObjectURL(blob);//建立本地链接
    image.src = objectURL;//图片元素赋值
```

参考: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Fetching_data

### 第三放api

看文档吧: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Third_party_APIs

### 画图

也是文档: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Drawing_graphics

### 音视频

文档: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Video_and_audio_APIs