> 本来呢, 为了开发编辑器, 关闭了tab转空格, 但是, 这玩意经常导致coffee出问题, 所以最终还是打开了

#### 改配置文件

```json
{
  "tab_size": 1,
  "translate_tabs_to_spaces": true,
  "detect_indentation": false
}
```





#### 如果一定要不转

- 那么设置特殊的文件
- preference->syntax - specific
