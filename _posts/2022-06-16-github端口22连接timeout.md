> git pull的时候忽然报错: ssh: connect to host github.com port 22: Connection refused fatal: Could not read from remote repository. Please make sure you have the correct access rights and the repository exists.





1. github添加key不起作用
```sh
ls -al ~/.ssh #列出所有key
# 不论是ed25519还是正常ras, 添加之后, 都没有用
pbcopy < ~/.ssh/id_rsa.pub

```
2. google搜索, 傻叉chrome一定要打开terminal, 删除了ssh后面的冒号才ok
3. https://stackoverflow.com/questions/15589682/ssh-connect-to-host-github-com-port-22-connection-timed-out
```sh
sudo lime ~/.ssh/config #这个报错, alias不起作用
sudo mate ~/.ssh/config
# 添加下面三行
Host github.com
 Hostname ssh.github.com
 Port 443
# 回到terminal
ssh -T git@github.com
# 验证成功之后, 中间要授权一次
gl # 这个会成功

```
4. 至此大功告成, 历时15分钟


### 后记
> https://gist.github.com/Tamal/1cc77f88ef3e900aeae65f0e5e504794

```sh
# This should also timeout
ssh -T git@github.com
# ssh: connect to host github.com port 22: Connection timed out

# but this might work
ssh -T -p 443 git@ssh.github.com
# Hi xxxx! You've successfully authenticated, but GitHub does not provide shell access.
# Override SSH settings
vim ~/.ssh/config

# Add section below to it
Host github.com
  Hostname ssh.github.com
  Port 443

# 再次检查
ssh -T git@github.com
# Hi xxxxx! You've successfully authenticated, but GitHub does not provide shell access.
```