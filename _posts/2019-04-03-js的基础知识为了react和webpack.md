> 为了react和webpack, 需要搞很多内容, 比如webcomponent和proxy

###### 基础中的基础

> 直接阅读英文原版吧, 中文翻译让人看不懂: https://medium.com/dev-bits/a-perfect-guide-for-cracking-a-javascript-interview-a-developers-perspective-23a5c0fa4d0d

```js
console.log(square(5));
 
var square = function(n) { 
  return n * n; 
}
//这个会报错, 因为这种形式square并没有被提升上去. function xxx()={}才会提升
```

1. 闭包是js封装的基础, 为了避免全局变量污染.

2. apply和call的第一个参数是上下文, 可以理解为this.

3. bind只有个一个参数上下文, 作用和上面一致.

4. 对象是键值对, 值的范围很宽泛, 函数也没有问题的. 

5. 但是, get和set的语法很神奇, 并没有冒号.  define property, https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty

6. object上面的一堆方法和属性要熟悉. js安排了很多静态方法和静态属性再上面.

7. 正则, 这个虽然我熟悉, 但是可以参考: http://www.rexegg.com/regex-quickstart.html

8. map reduce filter 

   ```js
   var arr = [[1, 2], [3, 4], [5, 6]];
   var flattenedArray = arr.reduce((accumulator, currentValue) => {
     return accumulator.concat(currentValue);
   }, []); // returns [1, 2, 3, 4, 5, 6]
   
   flattenedArray;// [1, 2, 3, 4, 5, 6];
   ```

###### 事件操作我还没有入门

> 这一篇的翻译还是不错的: https://segmentfault.com/a/1190000018257074
> 非常难得的是这个翻译结合了原文中的几个引文, 做了整体的翻译.
> https://medium.freecodecamp.org/3-questions-to-watch-out-for-in-a-javascript-interview-725012834ccb

1. 直接绑定,  这个案例写的也比我写的高效, 直接拿到了元素数组.
2. 事件委托, 这个更合理了. 事件就绑定再父元素上面, 然后用e.target来处理.

```js
//直接绑, 会有问题, 页面上面绑了太多的事件.
document.addEventListener('DOMContentLoaded', function() {
  let app = document.getElementById('todo-app');
  let itimes = app.getElementsByClassName('item');

  for (let item of items) {
    item.addEventListener('click', function(){
      alert('you clicked on item: ' + item.innerHTML);
    })
  }
})
//委托的写法
document.addEventListener('DOMContentLoaded', function() {
  let app = document.getElementById('todo-app');
  app.addEventListener('click', function(e) {
    if (e.target && e.target.nodeName === 'LI') { 
      //还可以使用element.matches语法
      let item = e.target;
      alert('you clicked on item: ' + item.innerHTML)
    }
  })
})
```



闭包和let我也没有入门

```js
//下面两段代码等效
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  // pass in the variable i so that each function 
  // has access to the correct index
  setTimeout(function(i_local) {
    return function() {
      console.log('The index of this number is: ' + i_local);
    }
  }(i), 3000);
}
for (let i = 0; i < arr.length; i++) {
  // using the ES6 let syntax, it creates a new binding
  // every single time the function is called
  // read more here: http://exploringjs.com/es6/ch_variables.html#sec_let-const-loop-heads
  setTimeout(function() {
    console.log('The index of this number is: ' + i);
  }, 3000);
}
```

事件如果频繁触发, 比如滚动这种就会造成抖动. 所以需要下面两个技术:

1. throttle 节流
2. debounce 防抖

```js
// fn是我们需要包装的事件回调, interval是时间间隔的阈值
function throttle(fn, interval) {
  // last为上一次触发回调的时间
  let last = 0
  
  // 将throttle处理结果当作函数返回
  return function () {
      // 保留调用时的this上下文
      let context = this
      // 保留调用时传入的参数
      let args = arguments
      // 记录本次触发回调的时间
      let now = +new Date()
      
      // 判断上次触发的时间和本次触发的时间差是否小于时间间隔的阈值
      if (now - last >= interval) {
      // 如果时间间隔大于我们设定的时间间隔阈值，则执行回调
          last = now;
          fn.apply(context, args);
      }
    }
}

// 用throttle来包装scroll的回调
const better_scroll = throttle(() => console.log('触发了滚动事件'), 1000)

document.addEventListener('scroll', better_scroll)
```

```js
// fn是我们需要包装的事件回调, delay是每次推迟执行的等待时间
function debounce(fn, delay) {
  // 定时器
  let timer = null
  
  // 将debounce处理结果当作函数返回
  return function () {
    // 保留调用时的this上下文
    let context = this
    // 保留调用时传入的参数
    let args = arguments

    // 每次事件被触发时，都去清除之前的旧定时器
    if(timer) {
        clearTimeout(timer)
    }
    // 设立新定时器
    timer = setTimeout(function () {
      fn.apply(context, args)
    }, delay)
  }
}

// 用debounce来包装scroll的回调
const better_scroll = debounce(() => console.log('触发了滚动事件'), 1000)
document.addEventListener('scroll', better_scroll)
```

两个方案由各自的问题:

1. debounce有一点问题, 如果用户一直频繁操作, 我们安排的响应就一直不出现. 这个也会让用户有功能失灵的感觉.
2. throttle的问题是, 反应会比较迟钝, 我已经不滚动了, 但是还是要1s之后才有反应, 我们知道一般用户的期望响应时间是0.2s. 对于电竞职业选手反应时间会更短.

解决方案就是结合起来, 比如: 

- 1s之内只要debounce能生效就让他生效, 
- 如果debounce拖过了1s, 那么throttle作为底线每秒也会给用户一个反馈.

```js
// fn是我们需要包装的事件回调, delay是时间间隔的阈值
function throttle(fn, delay) {
  // last为上一次触发回调的时间, timer是定时器
  let last = 0, timer = null
  // 将throttle处理结果当作函数返回
  
  return function () { 
    // 保留调用时的this上下文
    let context = this
    // 保留调用时传入的参数
    let args = arguments
    // 记录本次触发回调的时间
    let now = +new Date()
    
    // 判断上次触发的时间和本次触发的时间差是否小于时间间隔的阈值
    if (now - last < delay) {
    // 如果时间间隔小于我们设定的时间间隔阈值，则为本次触发操作设立一个新的定时器
       clearTimeout(timer)
       timer = setTimeout(function () {
          last = now
          fn.apply(context, args)
        }, delay) //这里不对, 这个delay如果和前面的判断条件的delay一致, 那么, 每次都会执行下面的策略, 并不合理. 比如delay 1s, 每次都是超过1s才会有反馈, 不论用户多快的停止操作.
    } else {
        // 如果时间间隔超出了我们设定的时间间隔阈值，那就不等了，无论如何要反馈给用户一次响应
        last = now
        fn.apply(context, args)
    }
  }
}

// 用新的throttle包装scroll的回调
const better_scroll = throttle(() => console.log('触发了滚动事件'), 1000)

document.addEventListener('scroll', better_scroll)

```

再次感谢原作, 真的很用心的翻译: https://segmentfault.com/a/1190000018257074

那么问题来了, 这个能用promise/await/async写法重写吗?