> python保存json文件, 中文的答案都太差了, 还是要看stackoverflow


### 保存
- 不废话, 上代码

```python
# 如果只是ascII字符可以用这个: 
import json
with open('data.json', 'w') as f:
    json.dump(data, f)

# 如果有中文或者其他非ascII字符, 用这个: 
with open('orderdata20220811.json', 'w',  encoding='utf-8') as f:
    json.dump(data, f,  ensure_ascii=False, indent=2)

```
> 顺便吐槽一下: 比python老的都已经是utf-8默认了, 只有python死抱着ascII作为默认字符集, 颇有windows遗风啊



### 读取
```python
with open('orderindex20220815.json', 'r') as f:
    a=json.load( f)

```