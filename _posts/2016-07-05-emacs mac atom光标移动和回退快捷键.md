> emacs的快捷键, 几乎是所有文本编辑器的基础, 也是mac os x的快捷键基础. 
>
> 原因是, 和vi快捷键相比, emacs的快捷键是无模态(anti-mode)的, 而macintosh之父jef raskin认为mode是万恶之源, 并且, 模态的快捷键在图形界面下本身也是几乎没有可操作性的.

# 光标移动

1. 简单移动
   - 上 ⌃ p
   - 下 ⌃ n
   - 左 ⌃ b
   - 右 ⌃ f 
2. 整行
   - 行头 ⌃ a, ctrl-a to move to the beginning of a line
   - 行尾 ⌃ e, ctrl-e to move to the end of a line.
   - 注意这个行头一般情况是可以识别头部符号的, 比如markdown的列表或者段落, 也就是说, 光标不会定位在最前面.
3. 单词
   - emacs本身是有的, mac下面也试验出来.
   - 需要在termimal里面设置, use ⌥ as meta.
   - 然后 ⌥ ← 左移一个词, ⌥ b如果键位不冲突也可以左移一个词, 比如terminal里面⌥ f就是可以的. 
   - 右移 ⌥ → , 或者⌥ f
4. 删除
   - 向左删除, 直接使用⌫就好了.
   - 向右删除, ⌃ d.
   - ⌃ l 清屏幕
   - ⌃ u清除到行首, 很神奇, 这个快捷键terimial有效, textedit无效.
   - ⌃ k清除到行尾, ctrl-k to “kill” an entire line
5. 取消执行中的命令
   - ⌃ c这个大家应该都知道吧.
6. 粘贴
   - ⌃ y   粘贴最后一次删除的内容, ctrl-y to “yank” it back
   - ⌘ c, copy
   - ⌥  ⌘ v, moving
   - ⌘ v, paste
   - ⌘ d, 直接原地多一个副本





### 后记

> 为了写这个文档, 我才去查了一些资料, 然后, 发现了一个神奇的事, os x 本机上没有对这套快捷键有任何介绍, 但是, 这套快捷键是有作用的, 这是消失的快捷键? 不过问了google之后发现苹果官网有, 
>
> 苹果官网: https://support.apple.com/zh-cn/HT201236
>
> 还是国内的网络环境问题, 导致苹果的随机(联机)帮助基本废了.  其实苹果的联机帮助也有, 但是, 苹果能不能聪明一点, 没有网络的情况下, 直接本机显示?
>
> ruby中国真神奇了: https://ruby-china.org/topics/1241
>
> 几乎可以称为快捷键大全: http://www.danrodney.com/mac/

# 导航快捷键

1. 通用导航:
   - ⌘[ 和 ⌘]
   - 大家都支持的方式.
2. 特殊情况:
   - ⌥ ⌘ ← 和 ⌥ ⌘ → 
   - 这个只有phpstorm支持, 而且表现还有点怪异.
   - 怪异之处: ← 之后 → 是空的.

# 妈, 窗口不见了

> 这个不是快捷键, 但是, 是苹果总会发生的问题, 窗口不知道到哪里去了

1. 菜单法:
   - windows-> zoom
   - 窗口-> 缩放
2. 菜单没有咋办:
   - 去系统设置改一下分辨率, 然后再改回来, 强迫苹果刷新界面.

# mac菜单要陪⌥ 

1. 按下⌥ , 菜单变样了, 很多牛逼选项出现了.



### android studio

1、Ctrl + O快捷键
查看代码的大纲 即类的方法列表。
参考: [http://www.jianshu.com/p/eefa51c81b01]

terimal 光标移动

- 结论是: alt b f或者← → 都可以.
- 但是, android studio的termial是一个诡异状态, 不可以用上面的快捷键.