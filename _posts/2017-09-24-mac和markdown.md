### 古代的markdown

> 这两个就是虞姬, 不过继续使用没毛病, 知道你觉得有毛病为止.

- 元古代: mou  # 很不幸作者是个骗子, 这个应用就是这么扑街的. [😎]
- 中古代: macdown # 很不幸 这个作者直接剽窃了上面一个作者, 并且还给软件改了名字. [🤦‍♀️]

### 现代的markdown

> 这些都是有人在用, 并且也还在维护的.

- 中生代: 
  - sublime 
  - textmate
  - atom
  - ulysis
- 新生代:
  - bear
  - typora

### 特别介绍下两个新生代

bear

> 总有人喜欢ulysis这种集中管理的方式, 用这种工具, 你可以完整的忘记文件夹和文件, 类似印象笔记, 锤子笔记这种, 但是, 这种工具的局限性也在这里, 当你需要用github的wiki或者page服务的时候, 咋办? 因此, 这类markdown, 注定是笔记类, 或者你把文章草稿都放在这里也行.
>
> 对于markdown的支持, bear比ulysis更胜一筹.
> 并且, 几个小扩展深的我心, 和我自己想出来的一模一样, 奶奶的, 

typora

> 以上列出的, 对markdown支持最好的就是这个了, 对的, 你没看错, 就是对markdown的支持. 
>
> 这里就算是大段代码也完全没问题, 高亮显示完美. 还有一点, 这货到现在都不收费, 真心良心啊, bear和ulysis都按日子收费了.

### markdown的格式

```markdown
# 代表目录, 
### 这就是个3级目录
- 无序列表
1. 有序列表
​``` 代表代码块, 可以指定语言, 实现高亮
​```sh 这个就代表了shell脚本的代码块  
> 这个代表引用, 注意这个不是代码块用的.
没了, 真的没了, 实在是想不起来还用到啥格式了. markdown就是这么简单.
```

### markdown的搜索和预览

```sh
# 安装qlmarkdown 可以解决预览问题
brew cask install qlmarkdown
# 刷新
qlmanage -r 

# 解决搜索问题
1. 下载一个文件: http://cdn3.brettterpstra.com/downloads/Markdown.mdimporter.zip
2. 将markdown.dmimporter移动到~/Library/Spotlight, 如果没有这个文件夹, 咱们要自己个建, 注意大小写Spotlight, 首字母大写
3. 执行一句:
mdimport -r ~/Library/Spotlight/Markdown.mdimporter
mdimport -L #这句话用来验证上面这个是否加入了, 如果没加入, 那么需要等待, 或者可以尝试做做第四步重建索引.
4. 最后一步, 还需要重建索引, 这个过程漫长而且会有各种不正常, 强烈建议这个时候不要操作mac, 建议可以出去玩玩啥的, 反正我等了半个小时还是没好, 然后我出去吃了个饭, 就好了. 大约总时间2个小时之后, OK了.
sudo mdutil -E /
mdimport -L #再次验证
```

### 