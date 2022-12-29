> 基本都是sass的书写问题

### 问题1:

```sh
# 编译时报错
<w> This selector doesn't have any properties and won't be rendered.
```

```sass
// 错误的sass写法
html
body 
  height: 100%
  margin: 0
  padding: 0
// 正确的sass写法
html,body 
  height: 100%
  margin: 0
  padding: 0
```

### 问题2:

```sh
# 报错
webpack error: This selector doesn't have any properties and won't be rendered.  null
```

```sass
 // :后面的空格有意义
#unimilk:hover
// 作为选择器, 冒号后面不可以有空格

  padding: 0
// 作为值, 冒号后面必须有空格
```

