> 很莫名的, 每当新建项目的时候, xcode就报这个错误.

### 网上查了一下竟然是git引起的

1. 这个结论让我很惊讶.
2. 那么我们就看看git吧.

### 查看git设置

```sh
git config #这里面会有一堆提示, 恰好没有如何查看的参数.
git config --list #看到了, 我的设置里面只有user.name, 没有user.email,  具体参考[git查看]
git config --global user.email "lo@163.com" #这个世界清净了.
```

### 总结

1. 苹果乱报错很让人惊讶.
2. git 没有list的参数提示, 也很让人惊讶.
3. 两个惊讶放到一起, 这件事值得做30分钟. 因为我们的大墙让我们google很困难.