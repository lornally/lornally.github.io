> 参考: https://stackoverflow.com/questions/46510192/change-the-theme-in-jupyter-notebook




```sh
# 安装theme
conda install -c conda-forge jupyterthemes


# 后续操作可以再terminal也可以在nodebook单元格直接执行



# 看看都有哪些theme
jt -l
!jt -l # 如果nodebook执行都要加上!, 后续不再说明

# 此时直接修改theme会不生效, 需要重启notebook的server, 简单的关掉server所在特terminal或者ctrl-c, 然后在conda界面把notebook再点击出来就可以了
# 之后的修改直接刷新chrome页面就可以了
# 修改theme
jt -t chesterish # 黑色的 蓝 非常清晰  推荐
jt -t   oceans16 #黑色 蓝灰 非常清晰  推荐
jt -t   onedork # 黑色 绿 非常清晰 推荐

# 暗色的
jt -t   solarizedd # 黑色 绿 简洁 对比度也不太够
jt -t gruvboxd # 黑色的 对比度有点不够
jt -t   monokai # 黑色的 俗

# 明色的
jt -t grade3 # 白色的
jt -t gruvboxl # 黄色的
jt -t   solarizedl # 黄色

# 回到默认
jt -r #回到默认theme
```


### 解决黑夜模式没有工具栏的问题

```sh
jt -t solarizedd -T -N -kl
# 解释: 
Toolbar Visible -T
Name & Logo Visible -N
Kernel Logo Visible -kl

# 如果在单元格运行, 想要永久有效, 用!
!jt -t chesterish -T -N -kl
```
