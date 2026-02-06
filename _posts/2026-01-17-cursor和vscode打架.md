> sh命令行才是王道

cursor会破坏vscode的配置, 导致f5无法运行代码.

此时的症状:

1. 调试运行是可以的.
2. f5不行
3. ctrl f5 可以
4. `Cmd+Shift+P` → 输入 "Python: Run Python File in Terminal" 这个也是可以的
5. RCC: Error , 启动就有, qtForPython报的.

###### qtForPython 

* 这是一个问题. 删掉这个扩展. 能清静一点.



###### 没卵用的清理缓存和重装

```sh
# brew操作重装没有用, 不论是:
brew reinstall
brew uninstall + install

# 1. 退出 VSCode（Cmd+Q）
killall "Visual Studio Code" 2>/dev/null

# 2. 只执行这行（清理调试缓存）
rm -rf ~/Library/Application\ Support/Code/User/workspaceStorage/*/ms-python*
```

###### 卧槽, f5之后, 报错的output的筛选器是: love2d launcher

* 删除所有love相关的扩展.
* 这个世界清净了



### 看到这种情况, 我不禁陷入沉思, 这是为什么呢?

kimi说: **根本原因：VSCode 调试器注册的"命名空间污染"**, love2d 扩展抢占 Python 调试，不是 bug，而是 **VSCode 扩展机制的设计缺陷 + love2d 扩展的实现粗暴**。



####### 2026-02-06 后记

macos上面的vscode的f5失效了, 但是调试, 和文件右边的三角形play按钮都是可以的. 为什么? 我已经禁用了所有的插件, 只打开了python和ruff

在 VS Code 中打开命令面板（`Cmd+Shift+P`）→ 输入 `Developer: Toggle Keyboard Shortcuts Troubleshooting`

然后, f5没有任何反应, 卧槽, 真的是macos的设置问题, 神啊, 要设置为f5是用f功能, 而不是音量调节等等. 这是一个macos+karabiner设置.  神啊.



