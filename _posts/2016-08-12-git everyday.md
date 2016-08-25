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

   ​

   ​

