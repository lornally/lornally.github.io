### 本地运行, 可以看到报错: 

```sh
bundle exec jekyll serve
```



确认的问题: 

6. 确认了一个问题 日期(2016-09-12-)后面不能有空格.

7. 很多时候, 编译需要时间. 编译正确, 显示就不会有问题.
   terminal, 设置默认尺寸.

   ​

### 我发现的错误: 

```jinja2
{  % post_url 2016-06-14-在github搭建blog-使用pages+jekyll %   }
这个链接必须是真实存在的.
```



### 参考

http://jekyllcn.com/docs/templates/#post-url