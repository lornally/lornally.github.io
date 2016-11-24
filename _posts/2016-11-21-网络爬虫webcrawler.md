## web crawler 网络爬虫

> 大神的网址: https://doc.phpspider.org

### 原文引用: https://doc.phpspider.org



编写PHP网络爬虫, 需要具备以下技能:

- 爬虫采用PHP编写
- 从网页中抽取数据需要用XPath
- 当然我们还可以使用CSS选择器
- 很多情况下都会用到正则表达式
- Chrome的开发者工具是神器, 很多AJAX请求需要用它来分析

1. xpath: 单独一个文档分析.
2. css: 单独一个文档分析.
3. 正则表达式: 单独一个文档分析.

### 数学上的等价

1. 一串符号 html
2. 对他们的处理 xpath
3. 处理他们的工具 正则
4. 对他们的描述 css

他们都是等价的.

- 函数式语言的高级之处在于, 他们可能脱离上下文无关. 
- 有元语言能力的语言, 才是真正的语言.

### 附加一系列的字符串

1. 去掉标签: strip_tags

## 不是很有用的细节, 以马蜂窝举例

### 计划

先扒了日本. 有了扒取信息的备选, 再去扒, 研究下信息抓取. 晓薇这边和老马已沟通坐标内容信息的抓取，准备从马蜂窝的日本300多个景点开始尝试，预计下周可以有个结果



### 资源: 

马蜂窝
http://www.mafengwo.cn/jd/10183/gonglve.html

### 目的地: 

日本, 300个节点坐标
韩国
新加坡
台湾

## 技术

### 基础知识

1. 网络爬虫: 网络蜘蛛（Web spider）也叫网络爬虫（Web crawler）[1]，蚂蚁（ant），自动检索工具（automatic indexer），或者（在FOAF软件概念中）网络疾走（WEB scutter）

   1. 可以限制mime类型.
   2. url规范化, 比如都转化为小写.
   3. 路径递增, 解析爬取得路径.
   4. 主题/局部爬取.

2. robots.txt, 

   1. 一般在网站根目录.
   2. 最关键是指定了sitmaps, 示例: 

   ```txt
   User-agent: *
   Disallow: /music/
   Disallow: /travel-photos-albums/
   Disallow: /lushu/
   Disallow: /hotel/*/?sFrom=*
   Sitemap: http://www.mafengwo.cn/sitemapIndex.xml //就是这句话指定了sitemaps
   ```

3. sitemaps, 示例: 

   ```html
   <sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
   <sitemap>
   <loc>http://www.mafengwo.cn/article-0.xml</loc>
   <lastmod>2016-11-14</lastmod>
   </sitemap>
   <sitemap>
   <loc>http://www.mafengwo.cn/article-1.xml</loc>
   <lastmod>2016-11-14</lastmod>
   </sitemap>
   <sitemap>
   <loc>http://www.mafengwo.cn/user-3.xml</loc>
   <lastmod>2016-11-14</lastmod>
   </sitemap>
   </sitemapindex>
   ```

   1. 很可能有多个sitemap
   2. 都会放到这个主sitemap里面
   3. 官网: http://www.sitemaps.org/zh_CN/faq.html



### 数据采集:

1. php采集
2. 火车头采集器
   1. 基于.net.
   2. 需要windows.
   3. http://baike.baidu.com/view/1248321.htm
   4. 官网: http://www.locoy.com
3. python采集



### html解析

> html parser html解析器​

### jsoup

1. 作者在stackoverflow: http://stackoverflow.com/questions/2168610/which-html-parser-is-the-best

Self plug: I have just released a new Java HTML parser: jsoup. I mention it here because I think it will do what you are after.

Its party trick is a CSS selector syntax to find elements, e.g.:

String html = "<html><head><title>First parse</title></head>"
+ "<body><p>Parsed HTML into a doc.</p></body></html>";
    Document doc = Jsoup.parse(html);
    Elements links = doc.select("a");
    Element head = doc.select("head").first();
    See the Selector javadoc for more info.

参考:

1. https://www.ibm.com/developerworks/cn/java/j-lo-jsouphtml/
2. https://github.com/jhy/jsoup/

### php采集案例:
- 原文:[http://www.epooll.com/archives/806/]
- 源码: https://github.com/owner888/phpspider
- doc: https://doc.phpspider.org
- 官方qq群: 147824717
- dom解析: http://www.cnphp.info/php-simple-html-dom-parser-intro.html
- https://github.com/bupt1987/html-parser


### python采集

1. http://www.lovelucy.info/python-crawl-pages.html
2. https://docs.python.org/3.4/library/html.parser.html
3. https://docs.python.org/2/library/htmlparser.html

### 更多采集工具

1. beautiful soap: https://www.crummy.com/software/BeautifulSoup/
2. https://en.wikipedia.org/wiki/Comparison_of_HTML_parsers


## 思路

1. 采集html
2. 分析html
3. 先看一个案例.

### 新思路

1. 拿自己以前访问淘宝的代码.  使用curl拿到页面.
2. 用php或者python遍历某个网站.
3. 用正则或者xpath分解一张页面..

## 大神思路

1. 简单点就file或者file_get_contents，复杂点就curl
2. curl函数有很多问题.
3. fire_get_contents的问题: 超时需要设置.
4. PhantomJS  这个不错, 模拟浏览器 操作 我都用这个   直接 用 dom的方式就能搞定 点击 填写  提交啥的
5. 模拟浏览器操作，为什么不用油猴子呢? firefox配合油猴子写js, greasemonkey.
6. chrome有tampermonkey

### 实战

1. 使用file.

   1. https: 几个事情要做, 参见reg.php和php命令行文档, 解决.

   2. Warning: preg_match_all(): Unknown modifier 'h' 

      ```html
      /<h1>(.*?)<\/h1>/is
      </h1> 这个地方的/竟然没有转义, 妹的.
      #正则相关都赚到了posts里面的[正则再起].
      ```

      ​