

### dict {'Michael': 95, 'Bob': 75, 'Tracy': 85}

```python
# 就是hash哈希啊
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
d['Michael']

# 设置值
d['Adam'] = 67

# 取值
# 安全的取值, 如果不存在就返回制定的值, 例如-1
d.get('Thomas', -1)
# 类似list, pop取值, 并且删除了这个key
d.pop('Bob')

# 判断key存在
 'Thomas' in d
  
# 遍历
for k in d:
 print(k, 'corresponds to', d[k])

# 循环key/value
d = {'x': 'A', 'y': 'B', 'z': 'C' }
for k, v in d.items():
   print(k, '=', v)
# d.items()其实是turple list: dict_items([('x', 'A'), ('y', 'B'), ('z', 'C')])

#To Iterate over the values
for value in dictionary.values():
    print(value)
```

### 

