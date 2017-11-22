### 在docker里面安装软件, 以zsh为例

```sh
yum search zsh #因为那个docker运行的是centos 如果是其他os, 请使用对应的安装包命令
yum install zsh
echo $SHELL #查看当前的shell
ps #也能查看.
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" #安装http://ohmyz.sh/
chsh -s /bin/zsh #设置默认zsh, 很奇怪, 貌似不起作用.
```

### 