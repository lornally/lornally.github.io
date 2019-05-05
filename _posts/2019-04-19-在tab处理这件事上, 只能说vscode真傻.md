###### 事情的起因是这样的

- 忽然发现我的eslint报错, 报两个错误:

```sh
expected indentation of 4 spaces but found 2 tabs
replace `↹↹↹` with `······`eslint(prettier/prettier)
```

- 然后, 发现我的vscode会自动把缩进格式化为tab. 自动的哦, 会把空格也转成缩进.

###### 回想了一下

之前, 是因为我做文本处理, 所以发生了一个bug: 无论如何不能替换文本中的tab, 仔细查了一下, 因为一个设置, 导致vscode自动把我代码中的所有tab都换成了空格, 不管这个tab是在字符串里面还是在正则里面. 这个设置就是:

- 使用空格做缩进. 

然后我果断的取消了这个选择, 然后, 就发生了上面的事, vscode自动用tab做缩进了, 

### 也就是说vscode其实给我们的选择是:

- 你究竟要tab, 还是要空格, 有他没我, 有我没他, 给个痛快话吧.

这个, 太霸道了…...

###### 最终, 一通乱改最后发现可以通过修改eslint的配置解决问题:

```js
		"indent": 0,
```

当然, 我还改了vscode的配置(或许这个修改是不必要的, 但是, 实在是懒得折腾了)

```js
	"prettier.tabWidth": 2,
	"prettier.eslintIntegration": true,
	"prettier.stylelintIntegration": true,
```



###### 万能的stackoverflow貌似有一个更稳的方案

```json
{
    "env": {
        "browser": true,
        "es6": true,
        "node": true
    },
    "extends": "airbnb",
    "parser": "babel-eslint",
    "rules": {
        "indent": [2, "tab", { "SwitchCase": 1, "VariableDeclarator": 1 }],
        "no-tabs": 0,
        "react/prop-types": 0,
        "react/jsx-indent": [2, "tab"],
        "react/jsx-indent-props": [2, "tab"],
    }
}
```

> 地址: https://stackoverflow.com/questions/40835041/use-eslint-with-airbnb-style-and-tab-react-js

###### 后记

第二天问题再次出现, 依旧参考万能的stackoverflow, 最终eslintrc的rules改成下面这样, 就完美解决问题了.

```json
"indent": [2, "tab"],
"no-tabs":0,
"prettier/prettier": 0,
```

用下面这套更清爽

```json
"indent": 0,
"no-tabs":0,
"prettier/prettier": 0,
```

