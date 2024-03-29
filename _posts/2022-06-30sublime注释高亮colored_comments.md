
## ------------------定义------------------

```python
# ! important
# * deprecated
# ? question
# todo todo
# fixme fixme
# //  //: undefined

```

## ------------------安装------------------
- 安装package: colored_comments
- 通过command-palette 配置插件(cmd shift p)
- 然后要手动配置颜色, 注意此时要配置两个颜色
 - 一般使用的color theme: twilightcyanide
 - markdown使用的: dracula
 
 ## ------------------设置------------------

### 设置, 可以不配
- 增加了忽略大小写, 并不重要, 因为有几个都是符号, 不存在大小写问题
- 注意, 这里如果改写某个标签, 一定要完整copy过来, 这样, 没修改的那些才能生效, 简单说, 就算没修改也要设置上
```json
{

    // This is the list of tags/identifiers you want to highlight
    // in your code.
    "tags": {
       "Important": {
            // The name of the scope being use in your color scheme file
            "scope": "comments.important",
            // The actual identifier used to highlight the comments
            "identifier": "!",
            // Enables sublime.DRAW_SOLID_UNDERLINE
            // Only noticable if outline = true
            "underline": false,
            // Enables sublime.DRAW_STIPPLED_UNDERLINE
            // Only noticable if outline = true
            "stippled_underline": false,
            // Enables sublime.DRAW_SSQUIGGLY_UNDERLINE
            // Only noticable if outline = true
            "squiggly_underline": false,
            // Enables sublime.DRAW_NO_FILL
            // This disables coloring of text
            // and allows for the outline of the text
            "outline": false,
            // Treats the identifier
            // as an regular expression
            "is_regex": false,
            // Enables ignorecase for the ideentifier
            "ignorecase": true,
        },
        "Deprecated": {
            "scope": "comments.deprecated",
            "identifier": "*",
            "ignorecase": true,

        },
        "Question": {
            "scope": "comments.question",
            "identifier": "?",
            "ignorecase": true,

        },

        "TODO": {
            "scope": "comments.todo",
            "identifier": "TODO[:]?|todo[:]?",
            "is_regex": true,
            "ignorecase": true,
        },
        "FIXME": {
            "scope": "comments.fixme",
            "identifier": "FIXME[:]?|fixme[:]?",
            "is_regex": true,
            "ignorecase": true,

        },
        "UNDEFINED": {
            "scope": "comments.undefined",
            "identifier": "//[:]?",
            "is_regex": true,
            "ignorecase": true,

        }
    }
}
```

### color theme配置
- 注意: 此处应该同时配置主theme和markdown单独指定theme, 如果指定了其他文件theme也要单独配置
- 我用了dracula和twilightcyanide
- settings syntax specific能查看单独设置了啥
- 选定文件获得焦点之后, 通过custom color scheme 就可以编辑对应的theme了
- 都使用下面这套就可以了
```json
{
 // http://www.sublimetext.com/docs/3/color_schemes.html
 "variables": {
 },
 "globals": {
  // "foreground": "var(green)",
 },
 "rules": [
  {
   "name": "IMPORTANT COMMENTS",
   "scope": "comments.important",
   "foreground": "#ffaaaa",
   "background": "rgba(201,222,238, 0.2)",
  },
  {
   "name": "DEPRECATED COMMENTS",
   "scope": "comments.deprecated",
   "foreground": "#ffaaff",
   "background": "rgba(201,222,238, 0.2)",
  },
  {
   "name": "QUESTION COMMENTS",
   "scope": "comments.question",
   "foreground": "#aaffff",
   "background": "rgba(201,222,238, 0.2)",
  },
  {
   "name": "TODO COMMENTS",
   "scope": "comments.todo",
   "foreground": "#aaffaa",
   "background": "rgba(201,222,238, 0.2)",

  },
  {
   "name": "FIXME COMMENTS",
   "scope": "comments.fixme",
   "foreground": "#aaaaff",
   "background": "rgba(201,222,238, 0.2)",
  },
  {
   "name": "UNDEFINED COMMENTS",
   "scope": "comments.undefined",
   "foreground": "#ffffaa",
   "background": "rgba(201,222,238, 0.2)",
  },
 ],
}
```
# --------------------特别说明---------------------
1. 很神奇, word wrap会影响这个的效果, 建议打开word wrap