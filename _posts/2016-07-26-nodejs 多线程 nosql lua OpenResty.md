## 引子

> nodejs 多线程 nosql? 
> http://stackoverflow.com/questions/23447413/websockets-php-ajax-javascript-refresh-client



## js & es2015

### js的参考

- http://www.ecma-international.org/memento/TC39.htm
- https://github.com/tc39
- https://github.com/tc39/proposals
- https://tc39.github.io/process-document/
- https://github.com/tc39/ecma262

### 入门

- http://huangxuan.me/2015/09/22/js-version/
- babel is a polyfill: https://babeljs.io
- es2015介绍: https://babeljs.io/docs/learn-es2015/
- v8介绍: http://www.75team.com/post/v8-es6-es7-and-beyond.html
- es7介绍: http://www.cnblogs.com/whitewolf/p/details-of-ES7-JavaScript-Decorators.html
- es7制度介绍: http://wwsun.github.io/posts/new-in-es2016.html
- 异步回调的一个特性, 据说不包含在es7里面: http://aisk.me/using-async-await-to-avoid-callback-hell/
- 2016 js开发: https://github.com/gf-rd/blog/issues/29
- 异步详解: https://blog.risingstack.com/asynchronous-javascript/
- 阮一峰的入门6: http://es6.ruanyifeng.com
- 阮一峰的入门5: http://javascript.ruanyifeng.com

### nodejs

- 入门: http://nodeapi.ucdok.com/#/api/assert.html
- wiki: https://en.wikipedia.org/wiki/Node.js
- w3入门: http://www.w3schools.com/js/
- 官网: https://nodejs.org/en/
- 中文: http://nodejs.cn
- 中文手册: http://nodeapi.ucdok.com/#/api/documentation.html
- 中文文档: https://www.gitbook.com/book/0532/nodejs/details
- 中文资料: https://github.com/youyudehexie/node123
  几乎啥都有, 有网络的时候浏览下.
- 中文社区: https://cnodejs.org


## rail & ruby

- 多线程: http://blog.teeceepee.com/blog/2015/10/05/activejob-and-sidekiq-introduction/

## openresty

- http://www.stuq.org/page/detail/557
- http://jinnianshilongnian.iteye.com/blog/2280928

> 摘要: 在探讨如何从零开始OpenResty开发之前，我们先来重温下什么是OpenResty，先看一张OpenResty官网的截图。看看OpenResty作者章亦春(agentzh)对它的定义。
>
> 我的理解Openresty = Nginx + ngxhttp_lua_module + lua_resty*；它是一个原生Nginx台上一个HTTP_LUA模块，再加上一系列Lua_resty模块组成的一个Ngx_Lua高性能服务生态。
>
> 在没有OpenResty的时候，基于Nginx开发高性能后端服务这是一件高大上的事情，没有很深的C语言开发功底，是做不了的。
>
> 要是想做好那更是得对Nginx整体架构有深入细致的了解，Nginx源码里那些三星级（ `***` ）四星级（` **** `）的C指针让多少英雄好汉望而却步。而且C模块的开发调试真是不简单。
>
> 下图是Python社区大名鼎鼎的“大妈”Zoom.Quiet总结的Lua开发和C开发Nginx模块的流程，多么走心的领悟。