> 很神奇, 成天报这种错误

```
Line contains inconsistent indentationCoffeeLint

[stdin]:29:3: error: indentation mismatch

```

###### 原因是vscode自动转化tab为空格.  vscode贴入时, 自动转格式也是这个原因.

```coffeescript
commonly used
editor: insert spaces

# json
"editor.insertSpaces": false,
```

###### 紧缩模式

```js
"editor.wordWrap": "wordWrapColumn",
"editor.wordWrapColumn": 100
```

###### 显示空格和tab

```coffeescript
renderControlCharacters, #勾选
renderWhitespace # 选择all
```

