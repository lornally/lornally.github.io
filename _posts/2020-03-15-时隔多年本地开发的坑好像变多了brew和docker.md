> 事务总是曲折发展的, 从不方便->方便->不方便->方便
>
> 比如一开始开发本地配置好烦, 很容易把机器搞崩溃->虚拟机好方便->虚拟机好慢->docker好方便->dock需要镜像.
>
> 再比如本地安装软件好烦啊->macports帮忙->macports安装好慢/太多->brew来了->brew被墙了好慢->国内镜像帮忙科大清华:)

### 先说brew

1. brew啥都装不上, 各种缓慢

```sh
#https://lug.ustc.edu.cn/wiki/mirrors/help/brew.git
#替换brew.git:
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

#替换homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

#科大管杀管埋  []~(￣▽￣)~*   \(^oོ^)/ 
#重置brew.git:
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git

#重置homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```

然后我们发现, 依旧很慢, ╮(╯▽╰)╭

```sh
#https://lug.ustc.edu.cn/wiki/mirrors/help/homebrew-bottles
#还要配置2进制源bottles O(∩_∩)O哈哈~
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

再次安装(*￣︶￣), brew你没救了, 就是这么无奈╮(╯▽╰)╭

```sh
#哎呀还要替换cask, 就不能一口气把话说完嘛? 给知乎@王不知大神点赞
# 替换 Homebrew Cask
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
# 回退
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://github.com/Homebrew/homebrew-cask.git
```

###### 汇总一下

```sh
作者：王不知
链接：https://www.zhihu.com/question/31360766/answer/749386652
来源：知乎

# 替换 Homebrew
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# 替换 Homebrew Core
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

# 替换 Homebrew Cask
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

# 替换 Homebrew-bottles
# 对于 zsh 用户：
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc

#还有一个思路, 出了问题可以换
#brew用curl下载，所以给curl挂上socks5的代理即可. 在~/.curlrc文件中输入代理地址即可。比如;
socks5://127.0.0.1:1080
#命令行设置也行
export ALL_PROXY=socks5://127.0.0.1:1080
unset ALL_PROXY
```

###### 终审判决 ⁽⁽ଘ(ˊᵕˋ)ଓ⁾⁾*

1. 不要用国内的源, 任何一个都不要用, 神烦. 

2. 最佳方式是给zsh加几个命令. 用的时候代理上.

   ```sh
   code ~/.zshrc 
   #加入如下内容:
   alias setproxy="export ALL_PROXY=socks5://127.0.0.1:1080"
   alias unsetproxy="unset ALL_PROXY"
   alias ip="curl cip.cc"
   ```

3. 此时会出现bug: Could not resolve host 127.0.0.1

   ```sh
   #原因是因为配置了~/.curlrc, 简单的删除就可以了, 并且原本的配置也是不对的. 
   socks5://127.0.0.1:1080 #这个是错误的写法, 好神奇, 我怎么就发明了这个写法呢?
   socks5 = "127.0.0.1:1080" #这个正确, 但是用了前面的方法就不要用这个了. 注意: 这里只能用双银行不能用单引号, 尤其要避免智能配对引号.
   ```

4. 有一个很好的参考: https://mashaz.github.io/2017/08/07/all-socks5/



### brew蹲完docker蹲🧘‍♀️（︶︿︶）

- 这个好像可以直接拉. 很慢. 后面试试打开代理.
- 阿里云有"镜像加速器": https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors
- 每个人登录查看, 我的加速器是: https://a5hmunh5.mirror.aliyuncs.com

两种配置方式:

1. docker toolbox 这个是啥?  还有一个概念docker-machine //todo
2. docker ->preferences->deamon->registry mirrors-> apply &restart  这个找不到了, 是我的docker太新了吗? 

总是上面两个都没成功. 

```sh
sudo echo  '{
  "registry-mirrors": ["https://<my-docker-mirror-host>"]
}' >> /etc/docker/daemon.json                                          

```

fuck 哦, 官方文档坑死人, 阿里云和docker携手坑人ヽ(｀⌒´)ﾉ, 官方文档的位置不在etc/docker. 

- 坑货一: https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors
- 坑货二:https://docs.docker.com/docker-for-mac/

```sh
ls ~/.docker
code  ~/.docker/daemon.json #录入下面的内容, touch, cat, echo, vi/nano/mate
```

还是Stack Overflow靠谱: https://stackoverflow.com/questions/38785991/docker-deamon-config-path-under-mac-os

```json
{
  "registry-mirrors": ["https://a5hmunh5.mirror.aliyuncs.com"] 
}//娘惹, 真的需要这个中括号 彡(-_-;)彡
```

重启docker

> 终于成功了, 速度飞起:)     ヽ(ﾟ∀ﾟ)ﾒ(ﾟ∀ﾟ)ﾉ  ✌️   ﾍ|･∀･|ﾉ*~● ✌️   (●´∀｀●)ﾉ  ✌️ (＾－＾)V
>
> 我容易吗? 2个小时了.......  

新建文件参考了: 

- https://isister.cc/posts/Create-File-In-Linux/
- https://linuxhandbook.com/create-file-linux/