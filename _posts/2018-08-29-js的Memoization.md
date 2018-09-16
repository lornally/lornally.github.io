> 偶然看到: https://blog.osteele.com/2006/04/javascript-memoization/

###### memorization

就是记住函数结果, 下次函数调用时直接用结果, 这个其实一直是我想弄得, 尤其是当初做Android的时候. 

prototype的用处出现了, 

1. 第一个用处: 扩展function

   ```js
   Function.prototype.memoize = function () {
     var fn = this;
     var cacheName = ... // create a unique property name
     return function() {
       var key = serialize(arguments);
       var cache = this[cacheName] || this[cacheName] = {};
       return key in cache ? cache[key] :
         cache[key] = fn.apply(this, arguments);
     }
   }
   ```

2. 第二个用处: 不至于作为对象的function先执行

   ```js
   Bezier.prototype.getLength = function() {
     var length = ... // expensive computation
     return length;
   }.memoize();
   ```


###### 然后上面的1和2可以合并为一行写法, 针对没有参数的函数

1. 关键是要理解prototype的执行期. 他是编译期运行的吗? 还是说运行期时的一个定义周期?
2. Stack Overflow上面是说, prototype的函数方法, 会被所有的实例公用. 



```js
Bezier.prototype.getLength = function() {
  var length = ... // expensive computation 由于是在prototype里面, 所以就执行一次
  this.getLength = function(){return length};
  return length;
}
```

```js
Bezier.prototype.getLength = function() {
  var length = ... // expensive computation
  return (this.getLength = function(){return length})(); //缩写
}
```

类似的技术可以防止所有的二次调用

```js
OSGradients.initialize = {
  OSGradients.initialize = function() {};
  ... // initialization
}
```

避免内存泄漏

```js
Bezier.prototype.getLength = function() {
  var temp = new Array(10000);
  var length = ... // expensive computation
  temp = null; // so the value won't be captured    实例: null的用处.
  // the following line captures length and work:
  return (this.getLength = function(){return length})();
}
```

```js
// Function.K(value) is a constant-valued function that returns
// value. 函数解决
Function.K = function(value) {return function(){return value}};
Bezier.prototype.getLength = function() {
  var temp = ...
  var length = ...
  // the following line only captures length:
  return (this.getLength = Function.K(length))();
}
```



要研究的内容是: js的生命周期