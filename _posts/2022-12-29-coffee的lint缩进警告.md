> 是lint的配置问题

### 问题1:

```sh
# 代码书写时的警告
Line contains inconsistent indentationCoffeeLint
```

```json
  "indentation": {
    "value": 1, //这里要和编辑器设置的缩进尺寸一致, 不然就会被警告
    "level": "warn"
  },
```

