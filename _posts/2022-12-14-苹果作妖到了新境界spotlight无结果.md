> 目录中spotlight搜素, 没有任何结果, 全局搜索貌似是OK的, 好奇怪的表现

### 症状

- 所有的目录都不正常

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
