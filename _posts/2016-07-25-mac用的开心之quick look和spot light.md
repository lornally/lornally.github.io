为了让我的quick look用的开心, 查了很多资料. wiki还是最靠谱的. : https://en.wikipedia.org/wiki/Quick_Look#/Supported_file_types_by_default

不过有些注意要点

1. 要quick look 生效, 需要 `qlmanage -r` .
2. 介绍插件: http://tidbits.com/article/16254


> 最后总结陈词: 是否有quick look生成, 要重启电脑, 等待几分钟, 看看负责quick的app是否有启用权限(就是点击执行试试看) 等等, 最终靠运气.

spot light在el captian变得巨难用, 

- 因为无法调整结果顺序了,
- stackoverflow有权威回答, 俩字: 没门. 
- 这事就要命了, 很多时候我只是想查单词, 
- 恰好那个软件不支持三脂点击.

然后, 这个时候, 我spotlight, 一堆没用的信息. 恨死人了.

1. 被逼无奈, 试试alfred.
2. 然后, 我也是醉了, 默认啥都搜不出来.
3. 然后, 问了谷哥, stackoverflow,  原来他的逻辑是给予关键字的, 这些关键字是可以设置的. 好吧. 大家各自设置吧. 其实, 我最烦这种vi mode了.


> 这是两个基础功能, 但是一堆坑.

## 问题

- spotlight, 不能索引某些文本文件, 比如: markdown. 
- quicklook, 不能看某些文件, 比如某些文本文件和mindmap(比如freemind的mm).

## 索引md

mac 索引 md , 可以安装qlmarkdown, 请参考: 

- [https://github.com/toland/qlmarkdown]
- [http://stackoverflow.com/questions/365669/how-can-i-make-spotlight-index-markdown-files]
- 然后发现貌似不起作用, 是否要重建索引?
  - [https://support.apple.com/en-us/HT201716]
  - 也可以命令行重建索引.
- cool 成功了.

还有另外一条路, 使用richtext的plist:

- [http://hiltmon.com/blog/2015/11/17/a-yosemite-markdown-spotlight-importer/]
- [http://brettterpstra.com/2011/10/18/fixing-spotlight-indexing-of-markdown-content/]

## mindmap(freemind)的quicklook

- 重新安装freemind, 但是, 依旧不行.

- 不搞了, 基本就是不行.

- 历史上成功过, 但是, 目前是不行的.

  ```sh
   qlmanage -p qulitest.mm #用这行代码就可以看到问题.
  ```

