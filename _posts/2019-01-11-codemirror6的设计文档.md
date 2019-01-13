> ​	https://codemirror.net/6/

这个版本codemirror回到了从前, 又改回了editable模式.

由此也坚定了我的信心, 就用editable了.

> marijn在开始一个项目的时候总是会写一个设计文档, 然后, 慢慢的, 这个文档就失效了.
> https://codemirror.net/6/design.html
> https://codemirror.net/6/

###### 先说说官网

1. 官网回顾了2011年时候的情况. 
2. haverbeke分析了目前codemirror基于隐藏的textarea的编辑方式面临了很多难以解决的问题(其实未必).
3. 通过prosemirror, haverbeke发现editable已经是一个很OK的事了.

###### marijn haverbeke的期望

- 可访问性
- 手机操作
- 原生的unicode处理能力
- 扩展能力增强, 目前基于回调的机制会比较混乱.
- 模块化
- 依旧很快

###### 要点

- 文档和dom互相转化, 这里是个大文豪, 难道是正片文档转化吗?
- position: sticky 可以用来设置行号



总结一下: 这个设计文档和之前的几篇比起来, 水太多了.

