

#### 小型todo

1. 写个小php解析json, 显示图片列表. todo.
2. gshock有几个小todo, 有空看看.

### 查看阿里云安全问题

- 云盾-暗骑士, 安全巡检功能.
- ecshop漏洞. 下线所有ecshop.

## 



## mac操作

### 设置safari的样式, 为ibook的灰色模式

- ibook的模式
- typora/atom模式 night,  one dark
- terminal模式 pro
- android studio模式darcula
- textwarangle: douce nuit 4, BBEdit dark



#### 删除status menu

icon in the taskbar i

Status menus
The right end of the menu bar contains status menus, which are represented by icons.

Right side of menu bar, showing status menus
These menus show the status of your Mac or give you quick access to certain features—for example, you can quickly turn on Wi-Fi or turn off Bluetooth.

You can add some preference icons to the right end of the menu bar. For example, you can add a Volume control icon. You can also add status icons for some apps when you install them.

To rearrange the status menu icons, hold down the Command key while dragging the icons. To remove an icon, hold down the Command key and drag the icon out of the menu bar.

/bin/launchctl unload /Library/LaunchDaemons/com.logmein.logmeinserver.plist 2>/dev/null euid:0 uid:0
/bin/launchctl bsexec 68 chroot -u 501 -g 20 / /bin/launchctl unload -S Aqua /Library/LaunchAgents/com.logmein.logmeingui.plist 2>&1 euid:0 uid:0
/bin/launchctl bsexec 68 chroot -u 501 -g 20 / /bin/launchctl unload -S Aqua /Library/LaunchAgents/com.logmein.logmeinguiagent.plist 2>&1 euid:0 uid:0
/bin/launchctl unload -S LoginWindow /Library/LaunchAgents/com.logmein.logmeinguiagentatlogin.plist 2>/dev/null euid:0 uid:0

##### 有空看看mac编程, 写个一键清理程序. 清理类似qq/虾米的无用状态图标.