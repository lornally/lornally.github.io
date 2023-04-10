> 一通处理, 当天没搞定, 还以为需要大搞, 其实真心等等就好了

1. yekyll官网看看如何升级, 本地升一下, 非常简单, 就是gem或者bundle升一下的事
2. github的pages的设置, 在项目的设置里面, 打开https
3. 还去阿里云看了dns, 这个解析确实是有问题, 我代理到国外就访问不到了, 是个神奇的局域网解析. 但是, 也木得办法
4. 此时, 还是不能访问的, 提示混合了http和https, 没关系, 等待一下(我等了一天), github那边pages编译好了就万事大吉了



> 总之, 这是一个很简单的事情, 但是, 他的反射弧太长, 牵涉面太广, 因此很容易耗费过多不必要的精力

### 2023-04-07 后记

> 上次处理并没有搞定
> 每次提交都会报警, 需要升级很多库
> bundle exec jekyll serve, 会报一堆错误

### 处理

```sh
# 先确认ruby情况
ruby -v
# 版本不对看brew 安装情况
brew info ruby
# 可能需要安装/升级bundle
gem install bundler   
bundle update --bundler
```



### 事件记录

询问chatgpt, 他的意思是, 更新本地的jekyll, 然后commit到服务端

```sh
# 翻到之前的blog: 2016-09-14-使用pages.....本地调试错误...md
bundle exec jekyll serve
# 此时报错bundle找不到
gem install bundler:2.2.4   
# 此时报错没有权限(其实此时应该反应过来的)
sudo gem install bundler:2.2.4   
# 安装之后
bundle update --bundler 
# 报错ruby版本过低
brew list ruby
# ruby的brew安装版本不低, 但是mac系统自带的那个很低
```

再次询问chatgpt

```sh
brew info ruby 
# 按照提示操作
ruby -v
# 此时没有变化, 重启terminal之后, 一切都正常了
```

然后发现还是需要处理jekyll: command not found: jekyll
询问chatgpt

```sh
jekyll serve
# zsh: command not found: jekyll
gem list jekyll
# 如果没安装那么需要安装
gem install jekyll
# 如果安装了, 确认下path
echo $PATH
# 如果没有, 那么添加到.zshrc
export PATH="$PATH:$(ruby -e 'puts Gem.user_dir')/bin"
# 然后，运行以下命令使更改生效：
source ~/.zshrc
```

并没有卵用, 继续chatgpt, 询问怎么找到gem包

```sh
gem environment gemdir
# /opt/homebrew/lib/ruby/gems/3.2.0
```

问题来了, Gem.user_dir和environment gemdir不一致, 咋办?

```sh
# 升级gem试试
gem update --system

# RubyGems installed the following executables:
	/opt/homebrew/Cellar/ruby/3.2.1/bin/gem
	/opt/homebrew/Cellar/ruby/3.2.1/bin/bundle
	/opt/homebrew/Cellar/ruby/3.2.1/bin/bundler
# Ruby Interactive (ri) documentation was installed. ri is kind of like man 
# pages for Ruby libraries. You may access it like this:
  ri Classname
  ri Classname.class_method
  ri Classname#instance_method
```

安装rbenv解决ruby环境问题

```sh
# brew 安装
brew install rbenv
# 显示版本
rbenv versions
# system, 系统自带的版本
# base, 基础版本
# brew提供ruby安装给rbenv
brew install ruby-build

# 这个可能可以:
rbenv install /opt/homebrew/Cellar/ruby/3.2.2

# 最终用的这个
rbenv install -l  
rbenv install 3.2.2  
rbenv global 3.2.2 
rbenv local 3.2.2  

# 查看gem位置
gem env
# 看到结果
- GEM PATHS:
   - /opt/homebrew/lib/ruby/gems/3.2.0 # gem位置
   - /Users/bergman/.gem/ruby/3.2.0 # 用户的gem位置
   - /opt/homebrew/Cellar/ruby/3.2.2/lib/ruby/gems/3.2.0 #brew的gem位置
# 最终不费劲了, 把下面这行加入.zshrc
export PATH="/opt/homebrew/opt/ruby/bin:/opt/homebrew/lib/ruby/gems/3.2.0/bin:$PATH"

```

依旧不可以执行, 确实是版本问题

```sh
sudo gem install bundler jekyll
bundle update

jekyll serve # 失败
bundle exec jekyll serve # 依旧失败
# 报错: Deprecation: The 'gems' configuration option has been renamed to 'plugins'. Please update your config file accordingly.
# 修改: '_config.yml' 所有出现的'gems'选项，并将其替换为'plugins'选项。

bundle exec jekyll -v # 此时是3.9.3
gem update jekyll  # 毫无反应, 啥都没装
# 官方建议 https://jekyllrb.com/docs/upgrading/
bundle update jekyll 
bundle update 
gem update jekyll.

# 要更新github-pages
bundle update github-pages
gem update github-pages
```

反思一下, 可能是我用的theme的问题

> 一个悲伤的故事, 恐怕只能放弃这个了..... 就这样吧

- 21:00 想要放弃, 看到曙光

```sh
# 如果ruby升级, 可能导致无法执行
bundle add webrick
# 然后就可以执行了
bundle exec jekyll serve
# 调试模式
bundle exec jekyll serve --livereload
# 参考: https://juejin.cn/post/7134178480727523364
```

- 然后作者指出, 模板需要单独更新

### 20230410

> 有一个猜测, 可能并不需要theme, 只要改_config.html就OK了

* 此时 lornally.github.io OK了, 但是, isuyu.cn不行
* 回到github, 项目的setting tab. 点一下
* sidebar 的section中的 code and automation -> 点击pages
* 页面中找到custom domain, 改为isuyu.cn, 稍等一下, 这个世界清净了