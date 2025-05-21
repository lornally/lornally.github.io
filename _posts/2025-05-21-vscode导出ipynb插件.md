> 写了一个导出ipynb的插件

* 在vscode插件项目, 设置syntaxes和theme就可以了.

###### 核心代码(具体缘由在后文介绍)

package.json

```json
 "contributes": {
    "themes": [
      {
        "id": "notebook-py4nb-theme",
        "label": "export彩虹关键字主题",
        "path": "./themes/theme_exportpy.json",
        "uiTheme": "vs-dark"
      }
    ],
    "grammars": [
      {
        "path": "./syntaxes/python_syntax.json",
        "scopeName": "source.python.export.injection",
        "injectTo": [
          "source.python"
        ]
      }
    ]
  },
```



syntax.json

```json
{
  "scopeName": "source.python.export.injection",
  "injectionSelector": "L:source.python",
  "patterns": [
    {
      "match": "^\\s*(#) (,)(e)(x)(p)(o)(r)(t)( .*)?$",
      "captures": {
        "1": {
          "name": "comment.line.export.1.python"
        },
        "2": {
          "name": "comment.line.export.2.python"
        },
        "3": {
          "name": "comment.line.export.3.python"
        },
        "4": {
          "name": "comment.line.export.4.python"
        },
        "5": {
          "name": "comment.line.export.5.python"
        },
        "6": {
          "name": "comment.line.export.6.python"
        },
        "7": {
          "name": "comment.line.export.7.python"
        },
        "8": {
          "name": "comment.line.export.8.python"
        },
        "9": {
          "name": "comment.line.export.9.python"
        }
      }
    },
    {
      "match": "^\\s*(#) (,)(t)(e)(s)(t)( .*)?$",
      "captures": {
        "1": {
          "name": "comment.line.test.1.python"
        },
        "2": {
          "name": "comment.line.test.2.python"
        },
        "3": {
          "name": "comment.line.test.3.python"
        },
        "4": {
          "name": "comment.line.test.4.python"
        },
        "5": {
          "name": "comment.line.test.5.python"
        },
        "6": {
          "name": "comment.line.test.6.python"
        },
        "7": {
          "name": "comment.line.test.7.python"
        }
      }
    }
  ]
}

```



theme.json

```
{
  "name": "Notebook Exporter Theme",
  "type": "dark", 
  "semanticHighlighting": true,
  "tokenColors": [
    {
      "scope": "comment.line.export.1.python", 
      "settings": {
        "foreground": "#fb61b9",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.export.2.python", 
      "settings": {
        "foreground": "#fb61e9",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.export.3.python", 
      "settings": {
        "foreground": "#fb91f9",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.export.4.python", 
      "settings": {
        "foreground": "#fbc1ff",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.export.5.python", 
      "settings": {
        "foreground": "#dbc1ff",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.export.6.python", 
      "settings": {
        "foreground": "#cbc1ff",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.export.7.python", 
      "settings": {
        "foreground": "#abc1ff",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.export.8.python", 
      "settings": {
        "foreground": "#8bc1ff",
        "fontStyle": "bold italic"
      }
    },
     {
      "scope": "comment.line.export.9.python", 
      "settings": {
        "foreground": "#abffff",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.test.1.python", 
      "settings": {
        "foreground": "#fb61b9",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.test.2.python", 
      "settings": {
        "foreground": "#fb61e9",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.test.3.python", 
      "settings": {
        "foreground": "#fb91f9",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.test.4.python", 
      "settings": {
        "foreground": "#fbc1ff",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.test.5.python", 
      "settings": {
        "foreground": "#cbc1ff",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.test.6.python", 
      "settings": {
        "foreground": "#9bc1ff",
        "fontStyle": "bold italic"
      }
    },
    {
      "scope": "comment.line.test.7.python", 
      "settings": {
        "foreground": "#abffff",
        "fontStyle": "bold italic"
      }
    }
  ]
}
```

# py4nb README

* 提取ipynb代码为py, 通过注释控制提取段落.

## 缘起
* 用tag提取代码操作繁琐, 且隐晦(tag没有直接展示)
* 且不灵活, 只能取整个格子, 而很多很简单的代码, 会把测试和代码写在一个格子里面


## 功能
* 在ipynb的格子中添加注释代码, 通过注释表明提取哪些代码
```python
# ,export 从下一行开始导出本cell. 注意: #号顶格, 中间空一格,  ,export前面有个逗号
# ,test 本cell剩余部分是测试代码, 不需要提取, 注意, 此后的export不生效!
```
### 命令
* 命令面板.
  * 打开命令面板 `ctrl+shift+p` 或者 `cmd+shift+p`
  * 输入 `py4nb` 选择命令: `export marked cells to python file`
* 快捷键
  * 提取代码:
  * "win": "ctrl+shift+e",
  * "mac": "ctrl+cmd+e",

### 语法高亮
* 建议使用语法高亮, 这样正确的export和test都是彩虹字, 醒目的标注出哪些格子会被导出, 使用语法高亮有2个方式:
1. 指定使用本插件theme: 这个就是正常的vscode dark加了语法高亮. 
2. 建议使用使用支持本语法高亮的theme, 例如:
  * zedme
  * ...
3. 也可以手动指定语法高亮, 参考代码: isuyu.cn     blog: 2025-05-21-vscode导出ipynb插件





## 附加功能: 统一python缩进(需要配置)
本插件还提供了一个格式化python代码的功能, 因为导出的python代码是从多个格子出来的, 因为可能缩进会非常不统一, 整体会非常松散, 因此提供了一个统一缩进的功能.
* 格式化python(需要选区和用户设置)      
  * "key": "ctrl+shift+f",
  * "mac": "ctrl+tab",
### 格式化python时, 需要一定的用户设置
* 为了和系统的快捷键不冲突, 因此有2个妥协
  * 用户要修改快捷键配置文件, 解除有选区的情况下的系统快捷键绑定.
  * 虽然都是全文格式化/紧凑化, 但是, 为了不冲突, 还是要求用户有任意的选区, 才能格式化/紧凑化
* 用户必须把tab转化为空格, 文件中不应该有tab, 格式化工具并不知道如何处理tab, 一个tab等于多少个空格是完全无法预料的事情. 虽然vscode会自动把tab转化为空格, 但是, 用户直接用vscode打开的文件, 还是可能会有tab的.

* 请用户修改快捷键, 避免系统冲突:
  * keybindings.json
```json
 {
    "key": "ctrl+tab",
    "command": "-workbench.action.quickOpenPreviousRecentlyUsedEditorInGroup",
    "when": "editorHasSelection"
  },
  {
    "key": "ctrl+tab", 
    "command": "-workbench.action.openNextRecentlyUsedEditorInGroup",
    "when": "editorHasSelection"
  },

```

## 格式化功能有Known Issues
* 不处理正则, 因此正则有注释符号或者字符串符号, 就会导致整体解析出错, 结果就不是预期结果.
* 字符串处理非常潦草, 
  * 并未考虑字符串中的字符串符号.
  * 并未考虑注释中的字符串符号, 以及由此引发的字符串中的注释, 
* 基于缩进的代码是无法部分处理的, 因为他的缩进是互相关联的, 部分处理大概率导致无法预期的结果, 所以每次都是整体处理全py文件. 
* 为了和系统的快捷键不冲突, 因此有2个妥协
  * 用户要修改快捷键配置文件, 解除有选区的情况下的系统快捷键绑定.
  * 虽然都是全文格式化/紧凑化, 但是, 为了不冲突, 还是要求用户有任意的选区, 才能格式化/紧凑化
* 用户必须把tab转化为空格, 文件中不应该有tab, 格式化工具并不知道如何处理tab, 一个tab等于多少个空格是完全无法预料的事情. 虽然vscode会自动把tab转化为空格, 但是, 用户直接用vscode打开的文件, 还是可能会有tab的.
* 虽然但是, 如果使用了format功能, 新的文件并不会直接覆盖, 而是放在旁边, 因此, 可以任意尝试format, 不必担心, 他不修改原文件.


## 关键字匹配的技术描述
```js
顶格# ,(export|test) 任意其他字符可选
正则匹配
/^\s*# ,(export|test)( .*)?$/
```
