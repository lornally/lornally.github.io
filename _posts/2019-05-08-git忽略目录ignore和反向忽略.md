> 参考: http://blog.sina.com.cn/s/blog_601f224a01012a9r.html

###### 忽略本目录的文件

```sh
.gitignore
#目录下建这个文件就OK了.
```



###### 反向忽略   — 不忽略

```sh
#这里忽略了所有.开头的.
.* 
# 记得这个要写在对应的忽略语法的后面. 
!.gitignore
#这里跟踪.gitignore. 注意, 要写在.*之后, 否则会被覆盖.
```



###### 一个例子

```

# 忽略所有 .a 结尾的文件
*.a  
# 但 lib.a 除外 
!lib.a
# 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO  
/TODO
# 忽略 build/ 目录下的所有文件
build/
# 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt 
doc/*.txt
```

