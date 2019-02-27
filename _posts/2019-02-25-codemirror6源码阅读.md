> 之前阅读了一下下, 发现typescript阅读不下去. 今天决定续上.

###### ts入门

- https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html
- 这个tsc之后, vs code就开始给我报错. 奶奶的.
- 在我看来ts真正的用处在于: 可以再接口定义上面发力, 解决js构建大型系统的架构设计问题的一部分.
- node的用处在于, 他引入了module的概念, 让js真的有可能构建大型系统.

举个例子

```typescript
class Student {
  fullName: string;
    //下面的三个public代表声明了三个property属性.
  constructor(public firstName: string, public middleInitial: string, public lastName: string) {
    this.fullName = firstName + " " + middleInitial + " " + lastName;
  }
}

interface Person {
  firstName: string;
  lastName: string;
}

//greeter要求他的参数满足person接口要求.
function greeter(person: Person) {
  return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);

```



###### 回到codemirror

问题出现了, 早期版本的codemirror总是会有各种编译问题. 

###### 当前最新版本的codmirror6

- 发现需要npm install
- 第二步再实验npm test

然后对于node的test做了一点扩展阅读. mocha是一个测试框架, cm6恰好使用了这个框架.

- mocha
- rollup
- ts-node
- ist
- style-mod
- w3c-keyname

###### 关于npm 值得重点说

被坑了, 之前一直是npm start , npm test这么用的. 其实这个是默认的缩略写法, 一共没有几个脚本支持的. 正常写法是: npm run watch-demo

###### 然后我发现codemirror6的完成度很低.

1. 中文完全不行, 除非是注释中使用中文.
2. tab没有处理, 按tab就会失去焦点. 

> 结论: 在当前情况下, codemirror6不是很有借鉴意义, 除非我去参与这个项目的开发.

###### 0226更新

- 很神奇, 睡了一觉之后, 我发现codemirror中文是好的. 神啊, 什么情况? 是我的幻觉吗? 太神奇了.
- 找到原因了, 本地跑的demo是OK的. 
- 线上的demo是有问题的. 

###### 分析

- 除了key事件之外.
- observer也被使用了.