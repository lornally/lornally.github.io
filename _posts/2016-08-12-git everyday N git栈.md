## git 本身就是一个技术栈

> 看了git pro, 觉得自己就不错了, 但是, 工作中总有各种问题, 而且, 这些问题还很基础, 终于认识到git pro 远远不够.
>
> linus原话: github的问题在于, 它鼓励了不合理的操作, 因此, 我们没办法在github上面维护linux内核.
>
> 简单的说, git pro对于git的理解其实是有偏差的.

## git 如何做代码审核



## git基础操作

> git help everyday 描述了git常用命令, 这里面依旧藏龙卧虎.

1. 如果一个没有commit的修改, 你又不能commit时, 那么有两个选择, 藏起来/抛弃

   ```sh
   git reset —hard  #抛弃未commit修改, 这个是必须的
   git clean -dfx #清除那些没有被版本控制的文件. 这个是配套的, 如果git status没有显示未控制文件, 那么可以不弄这句.
   git stash #把本地未提交的东西保存一下. 免得checkout不同分支时会被混掉, 我就发生了这事. 血的教训啊.
   #stash 是一个栈, 可以pop出来, 也可以直接抛弃掉.
   # 类似下面的三句话, 可以简化
   git stash
   git checkout some_branch
   git stash pop
   git checkout --merge some_branch #这是简化版.
   ```

2. 很神奇的, git add . 会经常失效, 这样git add之后, commit 就不起作用了.

   - 当有删除文件的时候, git add .就不起作用了.
   - 需要git add -A 
   - git add .  管理新的和变化的, 不管理删除rm的.
   - git add -u 管理变化的和删除rm的, 不管理新的.
   - git add -A管理上面两类.
   - 注意随着git版本不同, git add . 的表现也不同, 以上表现是git 2.9的表现.
     git

   ```sh
   git add .  #不起作用.
   git commit -am #起作用.
   #参考 慕课网的git: [http://www.imooc.com/article/1426]
   ```

3. git冲突

   - ==== 号上方是当前最新一次提交，下方是冲突的代码。
   - 从入门到放弃: [http://www.w3ctrain.com/2016/06/26/learn-git-in-30-minutes/]
   - git diff 作用: [http://ithelp.ithome.com.tw/articles/10135441]

4. git对比 这个要再看看.

   - add之前 show就可以
   - add之后 
   - commit之后 git diff

5. git指定远程源为默认源

   ```sh
   #改变git的default remote.
   gp -u <remote_name> <local_branch_name>
   git push #就是gp
   ```

   ​

6. git真对远程的rebase操作 

   ```sh
   gf xxx #git fetch 远程源xxx
   git fetch #这个就是gf
   grb xxx/master #rebase xxx源的master
   git rebase #这个就是grb
   ```

   ​

7. 查看某个文件所有的版本.

   ```sh
    git log -p filename #这个命令真的看到了代码变化.
    git log filename #这个只是历史. 没有代码变化.
    git log --pretty=oneline 文件名
   # [http://www.binkery.com/archives/203.html]
   git log --follow -p file
   git log -- file-name 
   git blame filename

   # http://progit.org/book/zh/ch2-3.html
   #详细展示一行中的修改
   git diff --word-diff

   #删除的内容如何找出来: 
   git log -S <string> path/to/file
   #例如: 
   git log -S setPicture_id app/src/main/java/com/takungae/dagong30/dgruning.java
   #这个blame方法不行, 据说可以试试bisect
   git blame --reverse START..END app/src/main/java/com/takungae/dagong30/dgruning.java
   #这一句最正确: 要加-c 或者 -cc
   git log -c -S 'setPicture_id' app/src/main/java/com/takungae/dagong30/dgruning.java 
   ####################################
   # 这个最正确
   ####################################
   git log -c -S '你要搜索的字符串比如删除的那一串' 你要搜索的文件路径/你要搜索的文件名.java 
   ```

   ​

8. 比较两个文件/文本. 文本比对, 文件比较.

   ```sh
   diff xxx ooo#这个其实最爽了, 如果有文件的话.
   beyond compare #只有文本也可以, 这个是我最喜欢的地方.
   ```

   这件事还有很多工具可以做.

   - diffmerge
   - beyond compare
   - Araxis Merge
   - Kaleidoscope
   - idea工具可以用.
   - xcode工具也可以.
   - meld

9. 查看所有被git忽略的文件.

   - 一句话解决问题: git ls-files --others -i --exclude-standard 
     - 又是ruby中国: [https://ruby-china.org/topics/17951]

10. 猴子学git: [http://backlogtool.com/git-guide/cn/intro/intro1\_3.html]

   这个依旧是gitpro思路在教, 不需要看了.



## git栈



1. linus说: [http://www.ido321.com/1669.html] 其中对github的评价值得深挖. 提交, 合并, 注释, 这些需要学习.
2. github说: [https://buzzorange.com/techorange/2016/08/11/github-intro/] 选择尽量少的技术栈.
3. codeing直播: [https://buzzorange.com/techorange/2016/08/11/why-we-should-go-livecoding/] 有空要做.
4. code review: [https://buzzorange.com/techorange/2016/08/16/airbnb-code-review/] 这个要做起来.
5. linux 内核: [http://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git]
   - [https://www.kernel.org]
   - 如何获取, 有空要看: [http://www.cnblogs.com/elect-fans/archive/2011/12/06/2408696.html]
   - 追踪技巧: [http://larmbr.com/2013/10/27/Git-for-linux-tips-for-tracking-code-history/]
   - git获取: [http://www.programgo.com/article/67463307945/]
   - 以上三分文档应该能解决我下面的git问题.