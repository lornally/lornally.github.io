> karabiner神经病, 非要修改function key, 不能不改吗?



### 快速解决 然并卵

- 本来想着, 怎么弄一个新的配置. 
- 快速解决是: 备份原来的配置文件, 然后直接改掉基础配置

1. misc -> open config folder
2. karabiner.json copy一个备份
3. fn_function_keys 删除成这样: 

```json
 "fn_function_keys": [
               
            ],
```

### 苹果也很神经

- 他竟然抢了f11这个快捷键在mission control



### 最终使用了最傻x的解决方案

- 在设置中尝试单独设置外置键盘的f键
- 然后, 打开配置文件, 手动添加所有的f键设置

> 纯纯的傻叉做法
