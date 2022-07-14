> 之前一直使用dark reader

### 问题
- darkreader其实不太好用, 对于图片, 透明等等处理的比较粗糙
- 自己写了一个插件, 但是, 维护起来太累了, 有三点: 
  1. 尤其是要和google这种明明支持dark, 却假装不支持的网站搏斗, 
  2. 并且要和chrome的安全机制搏斗, 他都不允许插件分析图片的. 实话说, 你不如禁止插件访问网络
  3. css变量也是一大阻碍, 并且html+css有很多hack写法, 尤其是很多论坛, 
    - 用背景色做border这种事茫茫多, 
    - 逐层嵌套背景也很难搞, 
    - 背景使用大纹理图片(是的, 他们不考虑页面加载速度的)......

### 疑惑
- 如果google不支持darkmode, 为什么她的所有图标都有dark和对应了的light呢?


### 解惑
> https://www.pocket-lint.com/apps/news/google/149866-how-to-enable-dark-mode-for-google-chrome

- 只需打开chrome, 地址栏输入这行命令
```sh
# 地址栏输入这个
chrome://flags/#enable-force-dark

```

- 然后可以抛弃dark reader