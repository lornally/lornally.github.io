> python需要处理数据的时候, 总是能够碰到空值, 很多时候这些空值是需要保留的, 并不能直接粗暴删除, 比如订单数据, 缺失了关键信息, 那么此时并不能删除这个订单, 而是, 留待后续从其他的数据源分析补充更多相关字段, 但是, 在进行运算的时候, 还希望空值也正常运算, 程序不要崩掉. 此时nan闪亮登场了



### nan
1. nan不是一个关键字, 因此不能直接用
2. nan有很特殊的性质, 很多时候可以利用这些性质来判断
3. nan属于true

```python

# 获得nan
float('nan')  
# 如果用了numpy
np.nan
# 用了math
math.nan

# 性质1 nan可以参与运算, 结果都是nan
float('nan') -1
# 性质2 nan参与的任何逻辑运算, 结果都是false
float('nan') >1

# 性质3 nan != nan
float('nan') == float('nan')
float('nan') != float('nan')

# 判断nan, 除了上面的!=之外
numpy.isnan(number)
math.isnan(x)

# nan 属于true
print('nan is true') if float('nan') else print('nan is false')

```
### none
1. none是一个关键字
2. none不能参与任何运算
3. 任何值都不可能是none, 除非你手动赋值他是none
4. 字典不存在的key, 可以得到value: none
5. "false" values include False, None, 0 and []
```python
# 得到none
d = {}
print(d.get('xx'))

# is 判断
float('nan')  is None
None is None

# == 判断
None == None # true
'' == None # false
float('nan') != float('nan') # true
```