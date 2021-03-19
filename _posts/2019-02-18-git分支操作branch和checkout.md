> 分支操作之前没有单独形成文档. 这里整理一下吧.

1. 新建分支. 本地新建, 远程新建.
2. 切换分支.
3. 删除分支.



###### 查询分支情况

```sh
gb #缩写
git branch

gba #缩写
git branch -a #所有的分支, 包括远程的.  = gb + gbr

gbr #缩写
git branch --remote
```

###### 本地新建分支

```sh
gcb #缩写, 以当前分支为基础创建新的分支, 并且切换过去.

#简单版的操作, 一步顶两步.
git checkout -b dev#本尊

#上面的操作等于下面两步:
git branch dev #这个不是切换分支, 这个是创建分支dev
gco dev #切换到dev分支.

#新分支beta推送到远程源origin
 gp origin beta
```

> 如果新建的分支远程没有, 那么可以直接往远程推, 就会推上去一个同名分支.

###### 本地切换分支

```sh
gco #如果是已有分支, 那么需要gco.
gcm #checkout master分支.
gcd #checkout develop分支.
```

> 如果切换的分支远程有, 那么自动会把远程这个分支再本地对应一个分支.

###### 把另一个分支的提交合并到本分支

```sh
grb dev #把dev分支的内容合并回来.
grb #默认是去远程origin的对应分支.
```





###### 错误的分支做了修改

```sh
#核心思路: 使用stash, 参考2016-12-06git每日
```



###### 错误的分支做了add

同上

###### 错误的分支做了commit

参考: 2020-02-07-git中reset和pushf真的有用

###### 删除分支

```sh
gb -d dev # 删除本地分支dev
git push origin --delete <BranchName> #删除远程分支
```

