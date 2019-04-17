> 学习compent之前貌似应该去了解下proxy.

###### 目的

- proxy可以给对象附加上函数, 配合get set, 可以控制原本对象的属性的访问.

###### 入门

- 实际上有个例子就是引用计数, 每次调用(一个目标)都通过proxy(多个), 这样就实现了引用计数. 如果proxy数量为0, 那么这个目标对象就可以销毁了.
- 他的语法和apply, bind, call很一致

```js
let p = new Proxy(target, handler);
//target是你要代理的对象.它可以是JavaScript中的任何合法对象.如: (数组, 对象, 函数等等)
//handler是你要自定义操作方法的一个集合.
//p是一个被代理后的新对象,它拥有target的一切属性和方法.只不过其行为和结果是在handler中自定义的.
```

- handler是为了代理设计的对象有13个原生方法, 包含get/set在内

###### 场景

- 可以拆分属性的判断.  测试环境有用.
- 可以用作缓存各种计算结果. 在路径计算之中应该有用. 仔细看这个例子并不对, 之前的计算并没有用上. 还是要用memoization, 话说, 是否能用es6重写memorization呢? 这个貌似是可以的,  而且真的可以, 这样就完美的避免了框架对语法的影响, 把框架降级为包了.
- 比如memorization的方案, 之前的好方案会造成framework, 会干扰正常的大计算消耗函数的写法. 用了defineproperty或者proxy, 就完全没有这个问题了. 对外暴露接口就可以把proxy暴露出去.



###### 相关

- decorator模式也可以做不少类似的事情.
- object.defineproperty做的事情很类似.

> 还挺清晰的, https://zhuanlan.zhihu.com/p/22584733
> 这个讲了defineproperty和proxy的异同: https://zhuanlan.zhihu.com/p/49952068

- 基本上可以认为proxy是对defineproperty的扩展. 