### 查看config

```sh
git config --list #很神奇的是, 直接git config 呼出的帮助里面没有list这个项目.
~/.gitconfig # 可以直接查看这个文件.
git config -l  #缩写方法.
git config -e #git config --edit的缩写, 用文本编辑器编辑.
```

```sh
git config --global core.editor "atom --wait"
#改编默认编辑器为 atom
```

