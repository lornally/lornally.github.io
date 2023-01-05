### 不错的教程

1. 言简意赅, 非常好: https://www.runoob.com/w3cnote/matplotlib-tutorial.html
2. 也不错, 不过是基于所谓"对象(并不是真正的字面量玩法)"的写法: https://zhuanlan.zhihu.com/p/139052035



### 样例代码

```python

# 这里引入另一个重要的库, 这个库用来绘图
import matplotlib.pyplot as plt
# 可视化看看
plt.plot(list(range(len(percentime))),percentime, label='percent')
plt.plot(list(range(len(absolutime))),absolutime, label='minute')
# 此处需要注意, 第一个参数是x数组, 这边只是弄了一个1...length的顺序数组

# 制定label的位置
plt.legend(loc='upper left')
plt.show()
```





### 中文问题

```python
plt.rcParams["font.family"]="SimHei"
```

