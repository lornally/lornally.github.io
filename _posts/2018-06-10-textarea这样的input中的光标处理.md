> 再input中, 某些时候我们要控制光标位置, 我们要格式化一个输入框.

光标

- you can use the properties `.selectionStart` or `.selectionEnd` (with no selection they're equal).

  ```js
  var cursorPosition = $('#myTextarea').prop("selectionStart");
  ```

- tab还是需要处理的. e.keyCode===9

  ```js
  document.querySelector("textarea").addEventListener('keydown',function(e) {
      if(e.keyCode === 9) { // tab was pressed
          // get caret position/selection
          var start = this.selectionStart;
          var end = this.selectionEnd;
          var target = e.target;
          var value = target.value;
          // set textarea value to: text before caret + tab + text after caret
          target.value = value.substring(0, start)
              + "\t"
              + value.substring(end);
          // put caret at right position again (add one for the tab)
          this.selectionStart = this.selectionEnd = start + 1;
          // prevent the focus lose
          e.preventDefault();
      }
  },false);
  ```

- 在myfield的光标处插入myvalue

  ```js
  var startPos = myField.selectionStart;
  var endPos = myField.selectionEnd;
  myField.value = myField.value.substring(0, startPos)
      + myValue
      + myField.value.substring(endPos, myField.value.length);
  ```

- 放置光标

  ```js
  txtarea.focus()//, add this line:
  txtarea.selectionEnd= end + 7;
  ```

- 一个著名的issue, textarea标签之间不能有空格!!!!!! 例如:

  ```html
   <textarea id='text' ></textarea> 这个没问题.
   <textarea id='text' > </textarea>这个就是问题了, 中间有个空格.
  ```

###### 参考知识点:

- 这个讲解了selectionrange, https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/setSelectionRange
- input的属性: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input
- https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement
- 关于selection: https://developer.mozilla.org/en-US/docs/Web/API/Selection
  - https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/setSelectionRange
- 这个网页是直接的讲解: http://help.dottoro.com/ljxjdqwh.php

###### 运转正常的我的样例代码

```js
text.value= `* textarea的cursor属性 
 * div的position属性
 * oeuaoeu
 * 事件机制
 * 在鼠标点击的地方放置光标div`
text.focus()  //这个很关键, 如果没有这个那么下面的内容不生效.
text.selectionStart=10
text.selectionEnd=20
text.focus()  //这个写在这里也可以的. 


```

