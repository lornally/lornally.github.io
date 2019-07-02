第一篇知乎专栏就是如何在github写blog,  没毛病. 

先说说github的好处: 

1. git的好处他都有.
2. 用git就够了, 提交就是gcam(git commit), 发布就是gp (git push). 
3. markdown格式就是目前最完美的大厂格式github格式, 虽然很多小众markdown格式更好(例如我记几葛儿的), 但是, 木有平台级支持啊. 



当然他也有缺点, 不然我为啥要来知乎bb呢,  唯一的缺点: 

1. 国内搜索不到, 很难和各位互动, 要互动也是在github互动, 



###### 闲话少说, 在github上面建blog拢共分三步:

1. 用你的username.github.io建立一个git项目**repository**, 比如: [lornally.github.io](http://lornally.github.io/)
2. clone这个项目到本地, 比如我: git clone https://github.com/lornally/lornally.github.io

```sh
#在根目录建立一个index.html, 不要太当真, 这个将来会被覆盖掉.
echo "Hello World" > index.html
#推送到服务器
git add .
git commit -m"first"
git push
#在浏览器可以看效果了:http://lornally.github.io
```

3. 没有了, 基础就两步. 这两步使用了pages. 可以参考github的相关文档.



###### 增强一下, 可以考虑采用jekyII, 很爽的.

> 关键的一点就是:  不要看github的说明文档, 不清楚, 去看jekyll自己的文档. 我就是在这个地方痛了一下.

1. 第一步, 安装, 这个地方是最容易发生问题的地方,  请翻墙, shadowsock都不一定好使, 我是vpn出去才搞定的. 或者如果你熟悉ruby, 可以尝试替换为taobao的源: [https://ruby.taobao.org](https://ruby.taobao.org/) , 淘宝靠谱的.

```sh
gem install jekyll
#继续在咱们之前的本地目录里面.
jekyll new . --force
```

2. 第二步: 这个时候, 已经可以到_posts目录写blog了, 注意文档格式有要求. 日期.md, **md** 代表markdown, 比如: 2009-04-26-我写了一个博客.md

3. 第三步: 没有了

```sh
# 看上一步的提示, 一般这样访问:   http://localhost:4000
# 打开serve就可以本地调试了.      
jekyll serve
# push到github, 就可以用http://lornally.github.io看效果了.
```

最后吐槽一下, 知乎这个编辑器真的是挑战人类能力的编辑器啊, 顺便说一句, 如果用github写blog, 那么可以用任意喜欢的编辑器, 比如: typora, vscode, sublime, atom, brackets, textmate, 任何一个都可以甩这个愚蠢的网页编辑器56769T条街......

> 后记, 最终我决定先在github写好, 再原文copy到这个知乎编辑器里面, 这个知乎编辑器, 真心搞不定.
> 最终我移到了掘金, 掘金靠谱的. 