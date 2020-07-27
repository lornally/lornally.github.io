> chrome extension的调试其实还是很方便的.

###### 单独调试

1. content调试
   1. devtooles, 左上角选择content-script
   2. console这里也是可以选择调试content还是页面
2. pop
   1. 直接右键就可以调试.
3. back
   1. 在插件页面可以点击back进入后台调试页面.

###### 注意:

- 这些调试页面也是页面, 也可能插件插进来, 尤其是后台调试页面

###### 通用方法:

- 用插件的id是可以调试代码的.
- 比如我的popup.html: [chrome-extension://plfilniddlhhfmhhcmganobhdjpjcmef/popup.html](chrome-extension://plfilniddlhhfmhhcmganobhdjpjcmef/popup.html)

###### 联调

- 后台页面可以在background里面直接改.
- 前台页面这边可以在console发消息给后台. 

###### 设置

- 如果想在调试的时候直接修改页面. 那么就要设置local source