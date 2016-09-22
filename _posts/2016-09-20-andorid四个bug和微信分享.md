> 这份文档, 是这个blog的续集: [2016-09-14-android使用intent调用系统activity返回result时, 本地的属性读不到的原因和分析]

## 引入类库

不论是啥类库包括微信, 引入方式都一样, 可简单了, copy文件到libs目录, 一切都ok了. 

```json
#一起都ok的原因是: android build.gradle 因为有这一句: 
dependencies {    
  compile fileTree(dir: 'libs', include: ['*.jar'])    testCompile 'junit:junit:4.12'    
  compile 'com.android.support:appcompat-v7:24.2.1'    		   
  compile 'com.android.support:multidex:1.0.0'}
#所以只要把jar放到libs目录就好了. 不必任何设置.
```

## 咱们先看一眼问题: 

### 硬件相关有两个问题:

1. 小米4.4.4兼容, luri为空. 就是要死磕这个.

   "此电脑不能读取您的磁盘", 小米针对windows的优化真彻底, mac根本读不出磁盘好不?

   那么连windows就没问题了吗? 不好意思, 我是猜的, 木有windows做测试哈. (羞愧)

2. 魅族6.0兼容, 无法调试. — 这个是魅族的bug, 呜呜, 再也不买魅族了.
   - 开发者选项貌似默认就是打开的.
   - usb调试
   - 安装外部应用
   - root
   - 以上这些都没有用, 升级android studio, 问题解决了. 神啊.

### 估计是硬件无关的三个问题:

1. 微信分享失败, 设置啥的都是正常的, 但是就是没反应.
2. 图片显示模糊.
3. 报错dalvikvm
4. 拍照搜索不正常
5. 我就是不识数, 咋办吧?......

## 思路, 去掉并行的线程试试.

1. 去掉多个并行的线程之后, 

   1. 依旧报错: VM with version 1.6.0 does not have multidex support

      关掉multidex, 这个报错就没有了.

   2. onBackPressed, 竟然被调用了.

   3. dalvikvm错误依旧存在. 妹的.

2. 1万头草泥马奔腾而去

   1. 就建一个最简单的android项目, 就会有这个问题: I/dalvikvm: Could not find method android.view.Window$

   2. 为啥想到要这么干呢, 是因为, 我在自己的项目里面注释, 发现是这句话引起了的问题: 

      ```java
      @Override
          protected void onCreate(Bundle savedInstanceState) {
              super.onCreate(savedInstanceState);//问题在这句话, 凌乱了.
              setContentView(R.layout.activity_main);
          }
      ```

   3. 然后, 我发现图库搜索正常的, 拍照搜索死活不行. 

3. 继续仔细看log

   1. 难道说, 隐式intent调用之后, 系统重新oncreate么?

4. 莫名其妙竟然好了, 我就是改了log而已啊!!!!!! log.d("d","xxxx"); 这个有啥问题吗?

   1. sourcetree看一下, 改了两个文件

      1. androidmanifest.xml文件去掉android:name="android.support.multidex.MultiDexApplication"

      2. mainactivity.java文件

         去掉一堆log, 改写一堆log

         oncreate注释了两个会发起图片下载的线程.

         onbackpressed注释了一个会发起图片下载的线程.

         onactivityresult每种code情况最后都添加了return. 

   2. 最关键的, 恐怕就是我升级了android studio.


## 认真的聊一下升级android studio这件事.

1. 之前所有的尝试, 都是失败.
2. 升级as之后, 魅族pro6好了.
3. 升级as之后, 小米拍照之后再切图也好了.
4. 其他问题待验证.

### 逐步验证

1. 放开oncreate的两个线程, 逐步放开.

   bug确实没了.

   切图之后依旧会崩溃.

   所谓的超出了api19的方法调用依旧存在, 恩, google不改也不知道为啥.

2. 测试微信分享是否也好了.

   没好.

   就这个问题了, 图片问题被我粗暴的回滚解决了. 

   恍然大悟, 之前gradle编译错误就报在微信上, 我咋没想到呢?

   二话不说, 从微信开放平台下载一个. 搞进去.

3. 神奇的事情发生了, bug在更早的地方, 但是, 为啥不喷log, 对了, 估计魅族也是需要设置的, 和华为一个德行.