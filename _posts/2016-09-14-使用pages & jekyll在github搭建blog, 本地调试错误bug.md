### 本地运行, 可以看到报错: 

```sh
bundle exec jekyll serve
```

没问题: 

1. , 肯定没有问题.
2. +肯定没有问题.
3. -肯定没有问题.
4. & 肯定没有问题.
5. 符号左右的空格不会引起问题.

确认的问题: 

6. 确认了一个问题 日期(2016-09-12-)后面不能有空格.

7. _ 这个有问题. do not use file or directory names that start with an underscore (_), period (.), or hash symbol (#), or that end with a tilde (~).

8. _.#~这四个符号有问题.

9. 很多时候, 编译需要时间. 编译正确, 显示就不会有问题.
   terminal, 设置默认尺寸.

   ​

### 发现的错误: 

```jinja2
{  % post_url 2016-06-14-在github搭建blog-使用pages+jekyll %   }
这个链接必须是真实存在的. 最好不要出现这种大括号和百分号.
```



### 参考

http://jekyllcn.com/docs/templates/#post-url