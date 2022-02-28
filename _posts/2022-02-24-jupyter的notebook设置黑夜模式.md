> 参考: https://stackoverflow.com/questions/46510192/change-the-theme-in-jupyter-notebook




```sh
# 安装theme
conda install -c conda-forge jupyterthemes

# 看看都有哪些theme
jt -l

# 此时直接修改theme会不生效, 需要重启notebook的server, 简单的关掉server所在特terminal或者ctrl-c, 然后在conda界面把notebook再点击出来就可以了
# 之后的修改直接刷新chrome页面就可以了
# 修改theme
jt -t   solarizedd # 黑色 绿 简洁


# 暗色的
jt -t chesterish # 黑色的 蓝
jt -t gruvboxd # 黑色的 对比度有点不够
jt -t   monokai # 黑色的 俗
jt -t   oceans16 #黑色 蓝灰
jt -t   onedork # 黑色 绿

# 明色的
jt -t grade3 # 白色的
jt -t gruvboxl # 黄色的
jt -t   solarizedl # 黄色

# 回到默认
jt -r #回到默认theme
```

