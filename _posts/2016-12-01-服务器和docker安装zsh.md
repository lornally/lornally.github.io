### 在docker里面安装软件, 以zsh为例

```sh
yum search zsh
yum install zsh
echo $SHELL #查看当前的shell
ps #也能查看.
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" #安装http://ohmyz.sh/
chsh -s /bin/zsh #设置默认zsh, 很奇怪, 貌似不起作用.
```

### 