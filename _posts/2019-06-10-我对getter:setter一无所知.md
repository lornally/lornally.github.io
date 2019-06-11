> 有必要把getter/setter整理到一起了.

###### 关于getter/setter 我从一开始就有一个误区



maybe i have find the solution, because i find a big lack of my cognitive concept.

- getter/setter is not method, it is properties.
  because this doc : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty

so we can do like this:

1. Object.getOwnPropertyDescriptors() or Object.getOwnPropertyNames()
2. then Object.defineProperties()



- 更多getter/setter知识
  - getter/setter在object层面是emumerable的, 但是在class层面不是.

```js
//测试下继承是否可以
class X {
  constructor(name) {
    this._name = name;
  }
  get name() {
    return this._name;
  }
  set name(name) {
    this._name = `${name}X`;
  }
}
class Y extends X {}
const yy = new Y('');
yy.name = 'hi';
log('yyyyyyy', yy); //真的可以继承
```

- getter/setter是可以继承的, 我之前的结论不对了. 这里可以看到很多层的proto嵌套.
- 这个博客讲的很透彻: https://nemisj.com/why-getterssetters-is-a-bad-idea-in-javascript/

###### getter/setter如何克隆

因为getter/setter 是properties, 所以我们要用clone properties 的思路来克隆他们.

- create可以clone, 
- assign和展开语法不可以. 
- 代码参见: js方法的克隆的方法,  在文档末尾与udercore的extend的新写法, 可以兼容setter/getter.