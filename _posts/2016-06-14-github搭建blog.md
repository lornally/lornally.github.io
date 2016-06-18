---
date: 2016-06-13 16:25:06
description: 忽然发现github可以搞blog, 太爽了.
tags: github, blog,  wiki, markdown, pages, jekyll, gem, 淘宝ruby.
---
### github搭建blog
本来有个3个想法:

1. 弄一下git, 学习一点git用法.
2. 找一个支持markdown的免费blog写写, 最好风格简洁, 适合程序员的.
3. 自建的wiki搭在阿里云上面, 果然在运转一年之后, 被黑掉了. 要搬家, 最好是分布式的. 避免悲剧.

然后, 发现这三个想法都在github实现了.

- gitbub自带wiki, 分布式, 而且几乎支持所有格式, 不论markdown还是wikimedia格式都通吃.
- github配合pages和jekyll完美支持blog. 不过官方文档的描述还是有一点模糊的, 因此我这里详细说说.

### 搭建blog要点
1. pages其实是自选项, 可以不弄, 不过如果弄了访问会很方便, 按照官方指引弄好, 你的blog地址就是: mckblog.github.io, 这个地址还是比较帅的. pages咋弄下面介绍.
2. jekyll要弄, 会遇到坑, 这些坑是中国特色, 大墙你懂的.
好了, 下面我就详细描述如何搞定blog.

### pages真的很简单, 仅需3步.
1. 用你的`username.github.io`建立一个git项目 **repository**, 比如我建立的就是: lornally.github.io
2. clone这个项目到本地, 比如我: git clone https://github.com/lornally/lornally.github.io
3. 在根目录建立一个index.html, 不要太当真, 这个将来会被覆盖掉.
echo "Hello World" > index.html
4. 推送到服务器
```bash    
git add .
git commit -m "first"
git push
```
5. 在浏览器可以看效果了:http://lornally.github.io

### jekyll也很简单.

#### 最重要的一点, 不要看github的说明文档, 不清楚, 去看jekyll自己的文档. 我就是在这个地方痛了一下.
1. 第一步, 安装, 这个地方是最容易发生问题的地方, 如果不行, 请翻墙, shadowsock都不一定好使, 我是vpn出去才搞定的. 或者如果你熟悉ruby, 可以尝试替换为taobao的源: https://ruby.taobao.org 详情请直接访问, 上面说的很清楚. 真的很清楚, 淘宝靠谱的.
```bash    
gem install jekyll
```
2. 继续在咱们之前的本地目录里面.
        jekyll new . --force
3. 这个时候, 已经可以到_posts目录写blog了, 注意文档格式有要求. `日期.md`, __md__ 代表markdown, 比如:
        2009-04-26-我写了一个博客.md
5. push到github, 就可以用http://lornally.github.io看效果了.
3. 打开serve就可以本地调试了.
        jekyll serve
5. 没有了, 看上一步的提示, 一般这样访问:    
        http://localhost:4000


### 参考
- 淘宝: https://ruby.taobao.org
- 中文: http://jekyll.bootcss.com/docs/posts/
- 英文: https://jekyllrb.com/docs/quickstart/
- pages: https://pages.github.com
- github help:  https://help.github.com/articles/user-organization-and-project-pages/#project-pages
- 还是github help:  https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/
- https://daringfireball.net/projects/markdown/
