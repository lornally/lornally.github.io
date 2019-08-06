> 
>
> 今天, github给我报了高危漏洞, 所以我要更新我的jekyll

```sh
#但是更新就报各种错
#比如说告诉你, 没啥可更新的.

gem update                                                           
# Updating installed gems
# Nothing to update

# 用brew安装ruby一样没用.

# 用bundle更是各种莫名.
bundle update 
# 下面这个报错都神了. 
Traceback (most recent call last):
	2: from /usr/local/opt/ruby/bin/bundle:23:in `<main>'
	1: from /usr/local/Cellar/ruby/2.6.3/lib/ruby/2.6.0/rubygems.rb:302:in `activate_bin_path'
/usr/local/Cellar/ruby/2.6.3/lib/ruby/2.6.0/rubygems.rb:283:in `find_spec_for_exe': Could not find 'bundler' (1.12.5) required by your /Users/bergman/Gemfile.lock. (Gem::GemNotFoundException)
To update to the latest version installed on your system, run `bundle update --bundler`.
To install the missing version, run `gem install bundler:1.12.5`
```

基本无计可施了. 

- https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll

  -  这个文档第二步, 第五小结下面的蓝色框框里面

    ```sh
    Tip: If you see a Ruby error when you try to install Jekyll using Bundler, you may need to use a package manager, such as RVM or Homebrew, to manage your Ruby installation. For more information, see Jekyll's troubleshooting page.
    ```

  - 这段话救命了, 我们去看一下

- https://jekyllrb.com/docs/troubleshooting/#jekyll-amp-mac-os-x-1011

  ```sh
  gem update --system
  ```

  - 针对mac就是这句话.
  - 然后再次bundle update, 一切都正常了. 

