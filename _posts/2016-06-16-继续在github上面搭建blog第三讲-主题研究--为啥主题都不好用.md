### 发现几个theme都不好用, 研究一下.

1. 貌似需要安装bundler

        gem install bundler
2. 建立一个gemfile, 内容:

        source 'https://ruby.taobao.org'
        gem 'github-pages', group: :jekyll_plugins
3. 安装jekyll:

        bundle install
4. yml配置文件, 注释两句话:

        # markdown:         redcarpet
        # relative_permalinks: true
   参考链接: https://kersulis.github.io/2015/10/31/jekyll-3/


### 关于theme
> 实验下来, 在两个theme中选择: `blackdoc/jekyll-clean-dark` 然后最好能和菩提树下一指禅zen结合一下, zen的字体和字号比较漂亮.

- 忽然之间, 配置好了官网的样式, 既然如此, 就用官网的吧, 这样就不必折腾了.
- 又发现了一个不错的网站 theme: http://startbootstrap.com
- blog的文件名不要用中文, 会找不到. 妹的.

### 目前发现, 我们要搬迁需要搬迁几个文件:
1. 配置类
2. config.yml
3. gemfile
4. posts下面的所有blog文件.


### 实验了一下jekyllbootstrap,
> 不好用, 12年之后就没更新了.

    git clone https://github.com/plusjade/jekyll-bootstrap.git lornally.github.com
    cd lornally.github.com
    git remote set-url origin git@github.com:lornally/lornally.github.com.git
    git push origin master



    rake theme:install git="https://github.com/jekyllbootstrap/theme-the-program.git"

### 官方theme
http://themes.jekyllrc.org
