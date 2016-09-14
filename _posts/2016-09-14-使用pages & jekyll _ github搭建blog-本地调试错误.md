### 本地运行, 可以看到报错: 

```sh
bundle exec jekyll serve
```

jellry有空看看, 出什么问题了. blog这个

1. 本地运行, 可以看到报错: bundle exec jekyll serve
2. {% post_url 2016-06-14-在github搭建blog-使用pages+jekyll %} 这种链接一定要写对.
3. & 不是这个问题.
4. _ 不知道是否这个问题.
5. , 不知道是否这个问题.
6. +肯定没有问题.
7. -肯定没有问题.

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