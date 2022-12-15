> 目录中spotlight搜素, 没有任何结果, 全局搜索貌似是OK的, 好奇怪的表现

### 症状

- 所有的目录都不正常

### 老办法

- 虽然此次症状如此诡异, 但是, 解决方案其实还是老办法
- 在setting里面, spotlight的排除目录, 全部排除, 再全部拉回来, 这个世界清净了
- 依旧没有清净, 考虑重装系统

### 最终mac回归了

- 他的表现像是20年前的windows, 重装解决一切问题, fuck

### log

- 重启corespotlightd, 在activity monitor
- 尝试设置中打开关闭, 并不解决
- 尝试: sudo mdutil -i on /, 并不解决
- 尝试重启
- 修复硬盘
- 重启SystemUIServer, 在activity monitor中
- 升级系统 13.0.1 -> 13.1
- 至此, 然并卵
- 而且还恶化了, 立着的显示器出问题了, sublime无法正常使用
- safe mode 试试
- 依旧, 然并卵
- 尝试: sudo mdutil -E /

