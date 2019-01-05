> ajax是相当古老的技术, 本身没有啥问题, 但是, 很容易让人联想到java, 也就是说辅助代码太多了.

###### ajax写法

```js
var request = new XMLHttpRequest();
request.open('GET', url);
request.responseType = 'text';

request.onload = function() {
  poemDisplay.textContent = request.response;
};

request.send();
```

###### fetch写法

```js
fetch(url).then(function(response) {
  response.text().then(function(text) {
    poemDisplay.textContent = text;
  });
});
//更好地写法, 避免括号地狱
fetch(url).then(function(response) {
  return response.text()
}).then(function(text) {
  poemDisplay.textContent = text;
});
```

