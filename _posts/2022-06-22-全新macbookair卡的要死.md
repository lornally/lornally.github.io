> 2020版macbookair, 五国语言, 苹果给换了新的, 然后卡死我了

## -----------------0701更新--------------
- 重装系统之后, 完全解决了, 
- 因此可以判断是icloud或者timemachine的问题
- 但是, 我也放弃了, 准备出售

## ------------------之前--------------------------
### 状态
1. timemachine在工作
  - 完了也没有影响, 排除
2. 钉钉, 这玩意也在工作
  - 退出后好了一点, 这个确实有影响
3. filevalt竟然是关闭状态
  - 打开之后好了一点, 这个确实也有影响
4. 电源状态1很怪, 在放电状态



### 高占比应用
- bird
- finder
- spotlight
- mds_stores


### 逐个尝试spotlight排除
1. 两个librery
2. system里面一堆, 此时, mds并没有降下来






### bird问题
> If Bird is using 100% of CPU on your Mac, check what files are causing the process to use too much CPU power and delete them. Then force-quit Bird and disable iCloud. If the issue persists, reset iCloud, create a new user account and reset SMC/NVRAM. Go to Keychain Access and delete any corrupted iCloud keys.

1. try this code, but fail
```sh
brctl log --wait --shorten
brctl log --w --shorten
```
2. stop icloud driver
```sh
25.65, 403,349item
```


### 还可以抢救一下
1. safe 模式, 重启, 按住shift键, 右上角可以看到safe boot
2. 三指和四指操作

### 没办法了, 做个启动盘吧
1. 先关掉icloud, 这样就会强迫上传icloud
```sh
sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/macos12

```
- turn on your Mac and immediately press and hold Command (⌘)-R until you see an Apple logo or other image.


### 过程中的抢救
1. 输入法尝试
```sh
pkill -f SCIM.app
```
奇怪, 似乎速度够快了

2. 待尝试
- PRAM/NVRAM & SMC



### 此时, 需要处理一下icloud问题
1. 有了一个思路, 关掉icloud, 但是, 保留文件, 貌似上传有效
2. icloud 依旧在疯狂上传

