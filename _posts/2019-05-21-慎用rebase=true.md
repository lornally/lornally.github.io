之前我的做法一直是设置rebase=true

```sh
[branch "master"] #pull的时候, 用rebase取代merge
rebase = true
```

但是当我gl的之后总是会莫名冲突, 然后又没有任何的显示的冲突, 

```sh
git rebase --abort
git fetch
git rebase #这么干一切正常.
```

所以, 之后还是正常的fetch/rebase, 或者

```sh
git pull --rebase
```

网上查了一下, gitconfig的新的写法换成了:

```sh
[pull]
rebase = true
```

这样或许是OK的, 等碰到了再验证下.