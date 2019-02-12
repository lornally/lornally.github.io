###### 拿到光标

```js
function getcaretposition() {
    return window.getSelection().anchorOffset;
}

```

###### 设置光标

```js
function setcaretposition(t) {
    window.getSelection().collapse(window.getSelection().anchorNode, t);
}
```

###### 事件

```js
    editor.onkeypress = (e) => { //特别神奇, onpress对中文兼容是完美的. 为什么? 我不禁低头问大地.
      let cp = getcaretposition();
      dom.value = doing(editor); //处理每键
      save.value = savefile(editor); //这个是save的做法.
      setcaretposition(cp);
    }
```

