> 再看胖箭头和funciton的时候, 忽然发现我其实没弄明白他们

###### 四个概念

- this
- bind
- 胖箭头
- function

```js
var x={};
funciton f(){};
x.xx=f;
x.xx(); //这个时候this就是x对象
x.xx=()=>{};//这里是有问题的, 箭头函数尽量不要作为方法.

var y={
	yy:()=>{}, //这里也是有问题的.
	yyy:function(){},
	yyyy(){},
}
```

###### mdn的描述很详细

- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions

> 大家一定要看英文文档, 中文文档各种不清楚和跟进不及时.

###### 我的理解



胖箭头这货我的感觉属于解决了一个问题, 但是会引起另一个问题的那类解决方案.

- 这个函数其实是一个弱化的函数, 已经不是js的一等公民了, 是一个传统意义上的函数了. 简单地说, 这不是一个functional的函数了.  这是一个纯粹的没有副作用的匿名函数.

- 号称他的this在声明时就确定了, 但是, 其实不是的, 调用方式还是有影响. 文档中的例子就说明了问题, 因此, 他依旧是在执行的时候确定this的.

  ```js
  const obj = {
    bar: function bar() {
      const x = (() => this); //这里只是把这个匿名函数赋值给x了, 并没有执行这个胖箭头的匿名函数.
      return x;
    },
    baf(){
      const x = (function x(){return this}); //正常函数.
      return x;
    },
    baff(){
      function xx(){return this}
      const x = xx; //正常函数更正常的写法.
      return x;
    },
    yy:()=>{return this}, //这里也是有问题的.
  };
  obj.xx=()=>{};//这里是有问题的, 箭头函数尽量不要作为方法. 因为obj自身是没有this的, obj.this===undefined.
  console.log('obj.yy()',obj.yy() === obj, obj.yy()); // false
  console.log('obj.xx()',obj.xx() === obj, obj.xx()); // false
  console.log('obj.this===undefined',obj.this===undefined); // true, obj的方法的this是obj, 但是, obj自身的this是undefined.
  const fn = obj.bar();
  console.log('fn() === obj',fn() === obj); // true, 因为在这里才执行. 作为obj的方法执行的. 如果这个不是胖箭头而是正常函数, 那么就不是true了, 因为正常函数, 不作为方法的时候, 他的this是全局window或者global
  const fn2 = obj.bar;
  // eslint-disable-next-line no-undef
  console.log('fn2()() == window',fn2()()===undefined ); //undefined, 这里就形成了一次直接调用执行, 没有作为方法. 
  const fnf = obj.baf();
  console.log('fnf() === obj',fnf() === obj, fnf()); // false, 正常函数, 这里应该是undefined了.
  const fn2f = obj.baf;
  // eslint-disable-next-line no-undef
  console.log('fn2f()() == window',fn2f()()===undefined ); //undefined, 这里就形成了一次直接调用执行, 没有作为方法. 
  const fnff = obj.baff();
  console.log('fnff() === obj',fnff() === obj, fnff()); // false, 正常函数
  const fn2ff = obj.baff;
  // eslint-disable-next-line no-undef
  console.log('fn2ff()() == window',fn2ff()()===undefined ); //undefined, 这里就形成了一次直接调用执行, 没有作为方法. 
  ```

- apply, call, bind的绑定this都不生效.

- 因为没有this, 一般情况下不能当method用.

- 因为没有prototype, 所以不能当构造函数用.

this

- 对象的方法有this, this就是这个对象.
- 一个函数本身是没有this的. 
- 正常函数和胖箭头的区别在于, 
  - 正常函数可以绑this
    - 通过apply, bind, call
    - 作为对象的方法直接声明.
    - o.xxx=ooo, 这样吧ooo函数赋值为o的xxx方法也是可以的.
  - 胖箭头不能绑this, 也不能作为对象的方法. 但是他可以再各种地方执行.
    - 函数里面. 尤其是setinterval/settimeout这种异步执行.
    - 方法里面. 
    - 然后, 他的this就是他执行的地方的外层的this了.
- 在dom里面, 用e.currentTarget, 更为合理. 因为this的表现依赖于引擎的实现.

> 总结陈词: 这么一通搞, 确实解决了很多问题, 但是, 为啥不干脆删除this呢?用类似e.currentTarget的方式来解决问题呢?

对象和函数的区别, 函数才是第一公民:

- 对象字面量没有函数作用域, 会丢失数据封装性.
- 这个就是为啥胖箭头作为对象的方法会没有this了. 因为对象自己是没有this作用域的. 