###### 改变原数组

```js
let map1 = [1, 2, 3, 4];
map1.push(...map1);
map1.unshift(4, 5)
```

###### 不改变原数组

```js
map1= Object.keys(map1).reduce(
  (accumulator, currentValue) => accumulator +','+ currentValue
  ,''
);
//arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])
```

- index 可选, 数组中正在处理的当前元素的索引。 如果提供了initialValue，则起始索引号为0，否则从索引1起始。

```js
map1 = map1.map(x => x * 2);
map1 = map1.flatMap( n=>
             (n < 0) ?      [] :
             (n % 2 == 0) ? [n] :
             [n-1, 1]
            );
```

- flatmap只扒一层皮.

```js
const pset=new Set([1,2,3]);
//array 转化为set很方便
pset.has(1)?pset.delete(1):pset.add(1);
//合并去重
arr=[...new Set([ ...a1||[],...a2||[]])];
```

- 配合set非常方便.