> 才发现, 某些事情, 我们想的太复杂了.

### github的密码自动记录 https方法

1. 网上有人说https不能缓存密码, 这个是不对的. mac和linux都支持https缓存密码, 甚至都是相同的工具, 不过是不同的参数而已.
2. linux: git config --global credential.helper cache
3. mac: git config --global credential.helper osxkeychain
4. 参考: https://help.github.com/articles/caching-your-github-password-in-git/

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
