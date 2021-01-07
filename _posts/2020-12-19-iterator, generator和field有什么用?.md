> 一直很疑惑, 都有闭包了, 为啥还要这些货

- 闭包closure回调可以做, 但是, 要显式的做, 并且可能还比较麻烦
  - 闭包可以保留环境变量, 但是, 无法保留调用栈, 因此try-catch就失效了.
  - generator可以暂停函数的执行来等待一个异步调用. 
  - 闭包需要显式的保留调用的返回值. 而generator的返回值保留是一个默认
  - 做简单的迭代的时候, 闭包也不够方便

- 省中间变量

```js
function* entries(o) {
  for (let k in o) yield [k, o[k]];
}
const x=entries({foo: 1, bar: 2})
const m = new Map(entries({foo: 1, bar: 2}))
```

###### 参考

- https://stackoverflow.com/questions/46119777/whats-the-difference-between-generators-and-closures-in-javascript
- https://github.com/luciotato/waitfor-ES6
- https://stackoverflow.com/questions/18842105/what-are-es6-generators-and-how-can-i-use-them-in-node-js

