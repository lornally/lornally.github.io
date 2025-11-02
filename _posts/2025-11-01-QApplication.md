> 需要理解下QApplication

* notify可以捕获的事件, 目标空间转发的, 所以filter和notify会捕获

| 事件类型                                          | 触发时机                                              | macOS 15 注意                                            |
| ------------------------------------------------- | ----------------------------------------------------- | -------------------------------------------------------- |
| `QEvent::TabletEnterProximity` / `LeaveProximity` | 使用 Apple Pencil（Sidecar 或 Studio Display 带触控） | 只在笔进入/离开屏幕边缘时发                              |
| `QEvent::Gesture` / `GestureOverride`             | 触控板捏合、旋转、智能缩放                            | macOS 15 默认会把三指 pinch 转成 `Qt::ZoomNativeGesture` |

* event可以捕获的事件(qapp自己的事件): 

| 事件类型（PySide 枚举）           | 典型触发场景（macOS 15）                                     |
| --------------------------------- | ------------------------------------------------------------ |
| `QEvent.FileOpen`                 | Finder 双击文档、拖拽文件到 Dock 图标、Spotlight 回车打开    |
| `QEvent.ApplicationActivate`      | 用户点击 Dock 图标、Mission Control 切回、Stage Manager 选回应用 |
| `QEvent.ApplicationDeactivate`    | 切到别的 App、进入 Mission Control、Stage Manager 隐藏       |
| `QEvent.ApplicationStateChange`   | 系统睡眠/唤醒、快速用户切换、控制中心→专注模式切换           |
| `QEvent.ApplicationFontChange`    | 系统设置→外观→字体大小变动（macOS 15 新“更大字体”滑块）      |
| `QEvent.ApplicationPaletteChange` | 深色/浅色自动切换、墙纸配色变动触发的外观色板更新            |
| `QEvent.Clipboard`                | **第一次**访问粘贴板、其他设备通过通用剪贴板推送内容         |
| `QEvent.LanguageChange`           | 动态加载/卸载翻译文件后（应用内），或系统多语言热切换        |
| `QEvent.LocaleChange`             | 系统设置→语言与地区→地区/24h/货币/温度单位改动               |
| `QEvent.SessionManager`           | macOS 关机/重启时请求“是否保存”会话（Qt 自动对接 NSApplication 回调） |
| `QEvent.Close`                    | 仅当**没有顶层窗口**时，用户 Cmd+Q 或 Dock→退出，事件直接发给 QApplication |
| `QEvent.Quit`                     | 调用 QCoreApplication::quit() 或最后一个窗口关闭且 quitOnLastWindowClosed=true 时发给自己 |
| `QEvent.Timer`                    | 你手动 `startTimer()` 在 QApplication 上建的 QBasicTimer 超时 |
| `QEvent.ChildAdded/ChildRemoved`  | 把 QObject 的 parent 设成 qApp，子对象被挂/卸时会进 event()  |
| `QEvent.DynamicPropertyChange`    | 对 qApp 自身调用 `setProperty()` 产生动态属性变更            |

* QApplication 有 event() 和 notify() 两个虚函数, 

  * event() 限于QApplication自身的事件处理

  * notify() 全局事件分发点，适合重载监控所有事件

    * 所有QObeject派生的都有这个notify

    * 但是只有app会自动生效, 其他的不在事件分发的链路时, 不会触发.

信号

| 信号原型                                              | 触发时机                                                     | 常见用途                     |
| ----------------------------------------------------- | ------------------------------------------------------------ | ---------------------------- |
| `aboutToQuit()`                                       | 事件循环即将退出（`quit()` 被调用或最后一个窗口关闭且 `quitOnLastWindowClosed=True`） | 保存设置、刷缓存、终止线程   |
| `lastWindowClosed()`                                  | 最后一个 `QWidget` 窗口被关闭（无论是否退出）                | 弹“确认退出”或隐藏到托盘     |
| `focusChanged(QWidget *old, QWidget *now)`            | 任意两个 QWidget 之间焦点切换                                | 全局记录当前焦点、更新状态栏 |
| `commitDataRequest(QSessionManager &)`                | macOS/Windows 会话管理器要求“准备关机/注销”                  | 保存会话、拒绝注销           |
| `saveStateRequest(QSessionManager &)`                 | 系统要求保存会话状态（休眠、快速用户切换）                   | 写恢复文件、注册重启参数     |
| `applicationStateChanged(Qt::ApplicationState state)` | 应用状态变化（`Active / Inactive / Suspended`）              | 暂停播放、停止网络轮询       |
| `screenAdded(QScreen *screen)`                        | 物理显示器插入或虚拟桌面新增                                 | 迁移窗口、重新计算 DPI       |
| `screenRemoved(QScreen *screen)`                      | 显示器拔掉                                                   | 把窗口挪回主屏、释放资源     |

```py
app = QApplication(argv)

# 1. 即将退出
app.aboutToQuit.connect(lambda: qDebug("about to quit"))

# 2. 焦点变化
app.focusChanged.connect(lambda old, now: qDebug(f"focus {old} -> {now}"))

# 3. 最后一个窗口关闭
app.lastWindowClosed.connect(lambda: qDebug("last window gone"))
```



- **事件** = 内部控制流，**必须同步、可拦截、立即消费**；

- **信号** = 对外通知接口，**松散、可选、可异步、多播**；

  

print此时很可能是失效/甚至是app崩溃的原因

* 用 **Qt 自己的日志通道** (`qDebug`, `qWarning`) 输出，保证在 Console.app / Xcode 日志 / `log stream` 里一定能看见。
* 开发阶段vscode的terminal直接可以看到
* app阶段, 打开应用: console, -> 点击工具栏最左侧的: 开始 -> 然后搜索框输入 xxx.app 作为过滤条件, 搜索框可以谓词
  * 直接输入: xxx.app
  * process: xx   只看xx进程
    * p: xx 缩写
  * `subsystem:com.mycompany` 只看子系统
  * `category:viewcycle` 只看类别
  * 组合多个键值, 用空格分割, 代表'且'关系

```sh
# 也可以用xcode工具来看
log stream --level debug --predicate 'processImagePath contains "YourApp"'
```

