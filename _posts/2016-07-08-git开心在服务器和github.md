> 才发现, 某些事情, 我们想的太复杂了.

### git效率, 提升效率是一定的.

1. 密码要搞定, 不必每次都输入. https和ssh方式都可以. 

   - 复杂的ssh方式, …… 需要把本地的~/.ssh/id_rsa.pub copy到服务器 ~/.ssh/authorized_keys文件中. 注意, 不是覆盖, 是把本地这一句, 放到服务器里面, 单独占一行.
   - 简洁的https方式. 可以一句话搞定.

   ```sh
   #复杂的ssh方式
   pbcopy <~/.ssh/id_rsa.pub #ssh方式用这个, 把这个公钥复制到服务器.
   profile setting->ssh key->add ssh key.#github 可以把这个公钥放到个人profile(个人首页右上角)里面.
   ssh-copy-id git@server #这个命令就是干活的, 但是, 需要 openssh-client package. 
   cat ~/.ssh/id_rsa.pub | ssh <user>@<hostname> 'cat >> .ssh/authorized_keys && echo "Key copied"' #这句话直接就可以用.

   #简洁的https方式
   git config --global credential.helper cache #linux
   git config --global credential.helper osxkeychain #mac
   ```

   ​

2. 服务器自动部署.

   - 服务器建一个裸体库, 比如mse.git
   - 本地添加远程源, 记得要推送本地需要推送的分支上去.
   - 服务器设置钩子, 自动更新工作目录

   ```sh
   #进入刚刚建立的裸体库mse.git/hooks目录, 然后编辑post-receive
   cd /data/www/gitm/bareserver/mse.git/hooks
   cp post-receive.sample post-receive #把sample改为配置.
   vi  post-receive#添加最后一行: 
   GIT_WORK_TREE=/data/www/gitm/workd/ git checkout weixin -f
   ```

   ​

3. rebase, 分支推主线之前, 先弄这个. 不要在push到服务器之后弄这个.

   ```sh
   git rebase master # 把master当做当前分支的基础, 这样master和store就没有分叉的历史了, 看上去也比较清爽, 而且也比较符合实际的业务逻辑. 这样当你推向主线的时候, 主线的管理者不需要merge了.
   git pull #rebase 之后需要pull.
   git push #rebase之后, 先pull, 再push. 然后发现整个目录清净了.
   #注意, push服务器之后, 就不要搞这个了. 这是在push之前做的.
   ```

   ​

4. 合并commit, 本地一堆commit, push之前合并一下.

   ```sh
   git add . #反复做这两步操作.
   git commit --amend -m "no need indepandent" #反复这两步, 提交就都提交到了最后一个版本上面.
   git rebase -i HEAD~3 #修改最近3次提交, 注意push服务器之前做这个, push之后再做这个, 就不合适了.
   #也就是说, git status时, 至少ahead 主线一个commit, 才能做这个操作, 否则就引起分支, 合并, 以及重复提交, 真心很烦, 会导致提交时间线比不整理更复杂, 且两次重复的一样的提交, 令人费解. 
   ```



### github的密码自动记录 https方法

1. 网上有人说https不能缓存密码, 这个是不对的. mac和linux都支持https缓存密码, 甚至都是相同的工具, 不过是不同的参数而已.

   ```sh
   git config --global credential.helper cache #linux
   git config --global credential.helper osxkeychain #mac
   ```

2. 参考: https://help.github.com/articles/caching-your-github-password-in-git/

### github的密码 ssh方法

> github右上角的profile setting->ssh key->add ssh key.

1. 本机制造一个key

   - 在terminal里面输入: ssh-keygen -t rsa -C "你的邮箱@qq.com"
   - 然后一路回车

2. 复制公钥可以直接用 pbcopy <~/.ssh/id_rsa.pub (感谢汤大师提供)\

   - 笨办法: 在terminal里面输入: cat ~/.ssh/id_rsa.pub


- 配合cat的步骤, 然后copy terminal里面显示的一大段喵星语,类似这个样子:
- ssh-rsa AAAAB3NzaC1yc2EAAAADAQABA你的邮箱@qq.com

3. 把公钥粘贴到github, 大功告成.

### git用户密码操作中, 需要检查的内容

1. 查看ssh的用户名, cat ~/.ssh/id_rsa.pub

2. 查看是否cache, 

   ```sh
   git config -l
   credential.helper=cache #看看是否有这一行, 有就说明cache了.
   ```

3. github https的用户名.

   1. 还是user.name和user.email.
   2. 没有是啥情况? 估计是传过ssh的公钥了.





### github的组织

1. 每次手工添加人很麻烦.
2. 可以建个组织, 然后把人都拉进来.
3. 组织也是可以改名的. 这个太爽了.
4. 具体操作也在个人设置里面.

### github双因素认证

1. 增加安全性可以双因素.
2. 手机安装google anthenticator.
3. 然后到个人设置里面, 双因素就好了.
4. 两点不爽.
   1. 支付宝不支持, 真心的, 没有kpi的事, 就是没人做.
   2. 短信验证, 不支持🇨🇳+86, 不知道为啥?
5. 第三点不爽, 导致我关掉了它.
   - 关于命令行如何使用, 官方介绍语焉不详, 貌似复杂. 

### git服务器自动部署

1. 服务器建立一个裸体库.

   ```sh
   git init --bare mse.git #mse.git是随便起的名字, 你的裸体库的名字, 自己可以随便起名
   ```

   ​

2. 本地添加这个裸体库作为远程源.

   ```sh
   git remote add hujingauto root@121.121.121.121:/data/www/gitm/bareserver/mse.git 
   #hujingauto是一个新的远程源.
   #root是server的用户名, 不该用root.
   #这个ip是server地址.
   #后面这一串地址是之前建立git裸体库时, git提示的.
   git push hujingauto weixin
   #推送到hujingauto, 用weixin分支到远程的微信分支, 这是分支提交的方法.
   ```

   ​

3. 服务器设置钩子, 自动更新工作目录.

   ```sh
   #进入刚刚建立的mse.git/hooks目录, 然后编辑post-receive
   cd /data/www/gitm/bareserver/mse.git/hooks
   cp post-receive.sample post-receive #把sample改为配置.
   vi  post-receive#添加最后一行: 
   GIT_WORK_TREE=/data/www/gitm/workd/ git checkout weixin -f
   ```

   ​

4. 为了安全可以考虑单独建用户. 对的这是第四步, 不过我没做. 汗......

   - 这个有点复杂, 有空再研究.

5. git指定远程源为默认源
   ```sh
    #改变git的default remote.
   	gp -u <remote_name> <local_branch_name>
   	git push #就是gp
   ```
6. git真对远程的rebase操作 

   ```sh
      gf xxx #git fetch 远程源xxx
      git fetch #这个就是gf
      grb xxx/master #rebase xxx源的master
      git rebase #这个就是grb
   ```
