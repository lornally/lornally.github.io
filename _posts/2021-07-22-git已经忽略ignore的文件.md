> 参考: https://stackoverflow.com/questions/466764/git-command-to-show-which-specific-files-are-ignored-by-gitignore



```sh
gst --ignored
git clean -ndX

# 下面这种并不可以
git check-ignore *
git check-ignore -v *
git ls-files -i
```

