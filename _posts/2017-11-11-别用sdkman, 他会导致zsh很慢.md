> 我的zsh最近忽然很慢了, 启动一下大概要10s

看了这篇文章:  https://bennycwong.github.io/post/speeding-up-oh-my-zsh/

然后测了一下, 我的系统就慢在了sdkman上面. sdkman真的遵循了java界一贯的情况.

1. 官方文档是错误的. 他官方说的如何complete(代码补全), 根本就是错的.
2. 各种缓慢. 
3. 然后, 官方安装之后要求我们手工在.zshrc做修改. 如果是ruby这么要求可以的, 但是, 他是一个类似brew这样的包工具, 竟然让我们手工改.zshrc.
4. 而且他给的修改方法还是错误的.

珍惜生命远离java, 包括java工具.

#### zsh优化

一般情况下, 参考我开头推荐的文章就可以了. 但是, 有的人还要追求极限, 那么可以看这个: https://carlosbecker.com/posts/speeding-up-zsh/

