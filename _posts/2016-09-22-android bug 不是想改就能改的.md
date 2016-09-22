本文是续集:

>  [2016-09-14-android使用intent调用系统activity返回result时, 本地的属性读不到的原因和分析]
>
> [2016-09-20-andorid四个bug和微信分享.md]

3大bug.

## 微信分享失败

一般情况下, 是码错了. 

```java
   wxapi = WXAPIFactory.createWXAPI(this, APP_ID);//这个app_id要check下.
```

但是, 我最近遇到的情况就是, 什么都不改, 2天之后这个好了. 

## 隐式调用intent之后, 宿主的onactivityresult里面无法读取本activity的属性

各种改, 各种调, 都没有毛用, 然后, 某一天自己好了. 初步确认是小米坑爹.

## gradle报错:Error:warning: Ignoring InnerClasses attribute for an anonymous inner class 

### 知乎和stackoverflow推荐的解决方案是: 

尝试在"proguard-rules.pro"中添加
-keepattributes InnerClasses
-dontoptimize
参见: https://www.zhihu.com/question/46649910

### 某网站说:

重复引用了jar

### 欲哭无泪的结论

1. 这个bug如果不是因为重复引用了jar, 那么是无法解决的, 又是android工具的事.
2. 重复编译, 多编译几次就好了.

## 在api level 19的手机使用最低版本19的apk包, 报错E/dalvikvm: Could not find class 'android.widget.Toolbar', referenced from method c

一般情况还跟着后面三个警告

W/dalvikvm: VFY: unable to resolve check-cast 2000 (Landroid/widget/Toolbar;) in Lcom/takungae/dagong30/MainActivity;
D/dalvikvm: VFY: replacing opcode 0x1f at 0x01bc
I/dalvikvm: Could not find method android.app.Activity.releaseInstance, referenced from method com.takungae.dagong30.MainActivity.access$super

### 结论: 无解, android本身的问题.

# 我们知道, 自己只是凡人, 很多问题, 我们无法解决, 可是怎么也想不到android有这么多[简单问题]无法解决, 哎. 然后, 这货还有这么大的市场占有率......