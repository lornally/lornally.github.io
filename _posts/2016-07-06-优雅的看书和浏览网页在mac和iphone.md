> 最近突然发现, 有了全套的mac, ipad, iphone….. 不代表可以优雅的看书上网. 这两件事啥时候变成了技术活.

#### 浏览网页

1. 如果使用safari, 那么需要注意两点
   1. 必须打开icloud的Safari选项, 否则不同步书签bookmark和favorite.
   2. 即便打开了, 也可能不同步, 因为, 必须在bookmark和favorite里面建立目录, 然后收藏的网页要放到目录里面, 否则等一万年, 也同步不了. 真神奇哩—对于苹果, 我已经无力吐槽了. 
   3. 搜索引擎用yahoo吧, 虽然不如google靠谱, 但是, 比百度还是强很多的.
2. 如果用chrome, 一切都好, 就是需要科学上网了.
   1. 配个vpn吧, 这个是我知道的唯一的手机上网的办法.
   2. 只要红杏出墙, 然后, 一切正常.

#### 看书

1. epub是王道, ibook默认支持的. 然后, 就没有然后了, epub放到ibook, 然后, 完全都不用做任何事了. 自动同步的.
   - 注意, 之前book是在itune里面同步的, 现在, 完全没有提示的情况下, 转移到了ibook, 我也比较佩服苹果, 不过ibook体验还是很不错的.
   - 然后同步了10本之后, 就停止同步了, 这个设置竟然还在itunes, 服了苹果.

2. calibre
   1. 如何获得epub? 我们可以转化,  https://calibre-ebook.com/download_osx
   2. stanza和marvin
   3. 这个转化epub的不灵啊, 吭哧吭哧转化了一宿, 然后, ibook打开能慢死. 不知道为啥? 书太大了?

3. 网络转化

   1. 顺便说一下, 这个转化有很多网站, 可以在线搞, 但是, 咱们这渣渣的网络啊, 我试了一下, "年轻人, 放弃吧, 除非你看这个文章时, 就在国外"
   2. 然后calibre不成功, 我被迫再次实验网络转化, 上午搞貌似还挺快的.
   3. https://cloudconvert.com/chm-to-epub, 这家要求你有云盘, 顺便, 我用了一下google的云盘.
   4. http://www.zamzar.com/convert/chm-to-epub/ 这一家会要求你输入一个邮箱, 结果会通过邮件发给你.



4. 也可以手机直接安装chm阅读器, 装了下面两个, 都是可以用的.

   1. chmate
   2. chm+



5. 还有第五条路可以走!!!!!!

   1. 自己制作一个epub文件.

   2. 下载文档为html格式, 一般都提供这个格式.

   3. 在文档的根目录建一个文件:  mimetype

      ```mime
      application/epub+zip
      ```

   4. 做一页封面: titlepage.xhtml, 记得里面放上封面图片.

   5. 建立一个索引页: toc.ncx

      ```xml
      <?xml version='1.0' encoding='utf-8'?>
      <ncx xmlns="http://www.daisy.org/z3986/2005/ncx/" version="2005-1" xml:lang="eng">
      <head>
      <meta content="0c159d12-f5fe-4323-8194-f5c652b89f5c" name="dtb:uid"/>
      <meta content="2" name="dtb:depth"/>
      <meta content="calibre (0.8.68)" name="dtb:generator"/>
      <meta content="0" name="dtb:totalPageCount"/>
      <meta content="0" name="dtb:maxPageNumber"/>
      </head>
      <docTitle>
      <text>How to Build a Website</text>
      </docTitle>
      <navMap>
      <navPoint id="a1" playOrder="0">
      <navLabel>
      <text>Hosting</text>
      </navLabel>
      <content src="build_website.html#step1"/>
      </navPoint>
      <navPoint id="a2" playOrder="1">
      <navLabel>
      <text>Do You Need a Domain Name?</text>
      </navLabel>
      <content src="build_website.html#step2"/>
      </navPoint>
      </navMap>
      </ncx>
      ```

   6. 在这一步就遇到了问题, 参考: http://webdesign.about.com/od/epub/a/build-an-epub.htm

   7. 我也想写一个转化程序了. 



