> 挺有病的, 这玩意要在多个地方设置, 参考: https://stackoverflow.com/questions/36814642/visual-studio-code-convert-spaces-to-tabs
>
> https://stackoverflow.com/questions/29972396/how-can-i-customize-the-tab-to-space-conversion-factor
>
> https://stackoverflow.com/questions/34174207/how-to-change-indentation-in-visual-studio-code

### vscode 的setting

- setting.json

```json
    "editor.tabSize": 1,
    "editor.insertSpaces": true,
    "editor.detectIndentation": false,
//特别注意这个false, 如果为true, 那么前面两个设置就会失效
```

### vscode的界面操作

- 界面右下角的边上space: 4
- 这个等同于:

```json
    "[coffeescript]": {
        "editor.tabSize": 1,
        "editor.insertSpaces": true
    }
```

### coffee的lint

- coffeelint.json

```json
  "indentation": {
    "value": 1,
    "level": "warn"
  },
```



