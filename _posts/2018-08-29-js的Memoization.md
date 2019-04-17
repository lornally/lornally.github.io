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
       var key = serialize(arguments);  //把参数变成key, 一会儿检查这个key是否计算过
       var cache = this[cacheName] || this[cacheName] = {};
       return key in cache ? cache[key] :
         cache[key] = fn.apply(this, arguments); //这个fn就是下面的调用函数Bezier
     }
   }
   ```

2. 第二个用处: 不至于作为对象的function先执行

   ```js
   Bezier.prototype.getLength = function() { //这个bezier就是上面的fn
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



要研究的内容是: js的生命周期 参见后面的blog, 我最终发现js的生命周期没啥可以研究的.

###### 改进

然后, 之前的这些解决方案有三个问题

1. 函数必须知道自己的名字, 也就是说每个函数都要再写一遍这个memory方法.
2. 如果有参数, 那么还是需要类似一开始非function.prototype的方式, 因为要检查这一组参数是否计算过.
3. 如果核心以来的私有参数有变化, 比如一个函数a可以设置参数给函数b使用, 那么函数b的memory就不能直接用上面的方法了.

```js
function Angle(radians) {this.setRadians(radians)}
Angle.prototype.setRadians = function(radians) {
  this.radians = radians;
  this.getDegrees.reset();//为了更新增加了这个代码, framework对语法形成影响就在这里了.
};
Angle.prototype.getDegrees = function() {
  return this.radians * 180 / Math.PI;
}
memoizeConstantMethod(Angle.prototype, 'getDegrees');

function memoizeConstantMethod(object, property) { //这个就是通用的记忆函数
  var f = object[property];
  var mf = function() {
    var value = f.call(this);
    var kf = function(){return value};
    kf.reset = reset;
    object[property] = kf;
    return value;
  }
  var reset = function() {
    object[property] = mf;
  }
  mf.reset = reset;
  reset();
}
```

这个方案并不完美, 因为他打扰了正常的setRadians的写法, 这就属于那种改变了语法的framework. 

###### 2019-04-10 更新

其实可以用包的写法解决, es6的object.defineproperty和proxy都可以解决这个问题, 完全不打扰angle的正常写法, angle完全不需要知道有memoize这种东西. 嘿嘿, 完美的解决方案是存在的. 