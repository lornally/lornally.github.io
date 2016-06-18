---
date: 2016-06-13 16:25:06
description: 接着前文, 继续用jekyll搭建blog, 我下载了一个theme.
tags: jekyll, blog,  theme,
---

### 不废话了直接上步骤.

1. 直接下载一个theme, 我用的 clean black, 另一个black: http://jekyllthemes.org/themes/blackdoc/
2. 把自己之前建好的blog里面如果已经写好了post, 记得copy出来. 然后, 清空.
3. 把下载的theme解压放进去, 然后, 记得把自己的文章放到_posts里面.
4. jekyll serve , 启动服务, 看清地址. 如果要改地址. 请修改config.yml文件.
5. 自己的post记得要有头部. 头部每行都有用:
>                //--- 这个是开头.
                layout: post, 这个是格式
                title: "继续在github上面搭建blog" 这个是页面title.
                date: 2016-06-17 16:25:06 这个是这篇blog预计上线的时间, 不到时间, 别人看不到
                description: 第二个测试页面. 如何用jekyll搭建blog, 我下载了一个theme, 测试一下 哈哈. 牛的, 上面的时间是开放出去的时间. //这里是描述
                tags: 这里都是这个blog要贴的标签.
                 - jekyll
                 - analytics
                 - tags
                 - comments
                ---
6.  其实在搞头部之前, 我们要改三个东西:
  1. __url: http://localhost:4000 __, 这个要指定你要在浏览器里面输入的url.
  2.   __baseurl: ''__, 这里要保持为空, 如果你是本地测试, 如果你的测试地址: http://localhost:4000/xxx, 那么这里就应该是: __baseurl: 'xxx'__
  3. 改掉defaults, 这样就不需要每个页面都指定布局了.
                  defaults:
                  -
                    scope:
                      path: "" # 一个空的字符串代表项目中所有的文件
                      type: "posts" # 以前的 `post`， 在 Jekyll 2.2 里。
                    values:
                      layout: "post"
7. 弄了default, 那么不使用default的页面就可能出问题, 咋办呢?.


### 各种推荐的黑色双栏theme:
- Naringu is a hacker like with dark theme for jekyll, 不好, 没有左边栏, 是単栏的.
- BlackDoc is a two-column black theme with a scrolling sidebar, 很不错的样子. http://jekyllthemes.org/themes/blackdoc/
- solar is A stylish theme for Jekyll blogs, based on the Solarized color palette. For installation instructions, please read the README.md on GitHub! 这个也不好, 左边只是汇总的, 没有文章列表, 不方便.
- Dark Kimochi 看上去就没有文章列表.
- Carte 还不错, 当前页展开比较爽. 不过展开之后就是白色的了.
- Jekyll Clean Dark  这个会有标签错误在汇总页面. http://jekyllthemes.org/themes/jekyll-clean-dark/
- madforjekyll 这个仅仅是图片展示, 摄影师会比较爽吧?
- saigon 也不好, 没有边栏.


# 难道一定要标题, 不需要.

- 改blog.
  1. 看看theme, 模板. 这个如果能解决是最好的.
  2. 看看自己弄html. 这个虽然灵活, 但是比较难, 而且弄漂亮需要天赋.



##### htmlNcss
1. 参考手册
2. target, base, iframe.
3. 其他解决方案. include咋弄?


再让我感慨一句: github太好用了.


* 小技巧, 看不懂,  先学习下基础, 再回头来看http://pizn.github.io/2012/03/01/some-tips-for-jekyll-blog.html
* 写模板: http://yansu.org/2014/02/12/how-to-deploy-a-blog-on-github-by-jekyll.html
* 官方模板引擎: https://help.shopify.com/themes/liquid/basics



### 放点参考资料吧:
- http://jekyllcn.com/docs/structure/
### 挑选模板的地方:
- jekyllthemes.org
- jekythemes.net
- mademistakes , https://mademistakes.com/work/jekyll-themes/
- http://startbootstrap.com/template-categories/all/
- https://github.com/jekyll/jekyll/wiki/Sites

配置 yml:
  title：网站名称。
  description：网站说明。
  logo：网站logo。
  disqus_shortname：disqus标示符。
  search：是否运行搜索。
  url：网站中一些资源文件使用的url地址。
  encoding：编码。
  markdown：md解析模板。
  timezone：时区。

配置自己的域名:
  在Github中的User Site Repo根目录（即Github中Jekyll目录的根目录）下创建CNAME目录，内容为你的个性域名，格式为www.yourdomain.com即可。

  然后在域名管理系统中解析域名，添加CNAME记录，服务器为username.github.io。等待一个多小时后，就可以使用自己的域名访问了。
