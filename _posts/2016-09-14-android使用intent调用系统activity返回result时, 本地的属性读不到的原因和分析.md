> 一开始, 发现或许oncreate中做的事务太多了, 因此, 无法做完, 导致onactivityresult中无法读到属性, 因此, 使用多线程解决问题.

```java
//主线程中启动新的线程, 就不写了, 很简单. 关键是在这个新的线程中, 我们要刷回主线作界面操作, 这个地方还是要写回调
((Activity)MainActivity.cma).runOnUiThread(//这个地方很关键, 佛祖啊, 在继承的过程中, runonuithread这事不知道被哪一层final了, 反正还原为自定义的activity就已经不行了. 忍不住要问google, 需要埋这么多坑吗?
  new Runnable() {
    public void run() {
      //这里写我们回到ui线程需要做的事.
      toast.show();                    }
  }
);
```

按照google的尿性, 会让我们这么'简dan单teng'的解决问题吗? 

## 当然不会了

google一定会让我们发现, 一切都不是代码问题. 这是我最痛恨android开发的地方, 最终我们一定会发现, 问题的根源是:

1. 各种设置.
2. 各种机型.
3. 各种google库冲突.
4. 各种...... 总之和你自身的代码质量联系微弱.

### 漫长的解决之路:

#### 1 移除Google Play Service/App indexing, 据说这个货有问题.

- 整件事都是google风格的. 怎么移除都汇报失败.
- 然后全局搜索上面这行英文的每一个字, 删除他们的code和注释!!!! 佛宗啊, 注释是关键, 阿弥陀佛. 祝google各位开发早日往生极乐.
- 然后, 重启google android studio 数次.
- 删除app/build文件夹数次, 是的就是你理解的, 直接删除.

佛祖啊, 最后在说一句, 为啥我要自己找病, 弄这个呢? 

- 这事也是google风格的, 
- 他说你的代码没有大问题, 但是有个小警告, 你要indexing一下, 
- 而且反复弹出这个警告, 正常人都会受不了, 点击: 一键处理, 
- 看明白了吗? google和360以及其他流氓是一样一样一样的, 
- 就这样还说自己不作恶, 呸.

然后, 这样会解决吗? 肯定不会啦, google还没玩够呢.

#### 2 著名的65k问题 

先介绍一下65k, 就是说, 整个项目不能超过65k的method, 

1. 这个限制很蛋疼, 

2. 尤其蛋疼的是, 这个问题通常是由于google库造成的, 一般人要写那么多method会吐血的, 只有google的高手才不会, 

3. 更蛋疼的是, google在api level 22 __"完美解决(shaXzengjia)"__了这个问题: 从此之后, 跨越level 21/22的应用程序就都会挂在这个上面.

4. 解决方案: 删除: google gms play-service, 恩, 这个貌似咱们刚刚做过.

   ```xml
   <!-- #在gradle的depandencies:   -->
   compile 'com.android.support:multidex:1.0.0'
   <!-- #在manifest   -->
   <application
           ...
          android:name="android.support.multidex.MultiDexApplication">
   <!-- 这是官方推荐的做法. 然并卵 -->
   ```
   ```java
   //据说如果你的application继承自multidex也行
   public class MyApplication extends MultiDexApplication {
       ...
   }
   //或者你重载attachbasecontext也行.
   public class MyApplication extends Application {
       @Override
       protected void attachBaseContext(Context base) {
       super.attachBaseContext(base);
       MultiDex.install(this);}
   }
   ```

   ​

5. 顺便说一句, 各种诡异问题, 不要忘记重新启动android studio, 诡异问题包括, 但是并不仅仅限于:

   - 修改配置没反应.
   - 调试代码没输出.
   - gradle一下子报几十个莫名的error.

#### 使用了超越level19的方法

1. 报错在哪里呢? android studio能否给点提示呢?

2. 据说lint可以报错

   - Analyze—>Configure Current File Analysis—>Configure Inspections 清空所有的检查项，然后勾选: 
   - Calling new methods on older versions
   - Using inlined constants on older versions

3. lint这么设置不起作用, 咋办?

4. 没关系, 还有gradle方法

   ```properties
   android{
       //some other config
       lintOptions {
           abortOnError false
           xmlReport false
           check 'NewApi', 'InlinedApi'
       }
   }
   #然后执行
    ./gradlew lint
   #我执行了这个, 然后, 就被迫去睡觉了.
   ```

5. 命令行方法, 这个最靠谱

   ```sh
   #项目根目录, 建议就在android studio的terminal里面运行.
   lint --check NewApi,InlinedApi --html ./tmp/api_check.html ./
   ```

   > 结论 lint没成功. 没有爆出任何错误.

6. 据说改gradle可以解决, 是google自己类库的问题.

   ```properties
   compile 'com.android.support:appcompat-v7:20.0.0'
   #尝试过了, 这个没有毛用.
   #改这个意味着一堆折腾, 因为后面的style也没有. 总之, 报啥错都注释, 然后把activity的继承改掉.
   不禁长叹一声, google的api level策略真心不行啊.
   ```

> 难道结论是, 我的代码没有使用超越level 19的api.
>
> 新建干净的项目, 已经确认是google自己类库的问题.

### 更多的结论

1. 一个说法是, 虽然我没有使用超越level19的api, 但是, google的support库用了.
2. 补充说法: 这是google level 23的特性. level23的surport库里面写错了. 和上一个说法不冲突.

### 尾声1: java sdk配置

后记, 中间其实还发现, 我虽然安装了jd1.6 1.7和1.8, 但是, android studio使用的竟然是1.6. 因此. 曾经怀疑是jdk的问题. 然后, 各种更新配置, 各种大墙, vpn, 科学上网. 折腾死了. 

1. 系统的java控制面板不正常.
2. android studio使用的是1.6的jdk.

```sh
/usr/libexec/java_home -V  #这个可以看到安装了那些java.
#在 ~/.bash_profile 中加入 JAVA_HOME 的設定, 这个地方我有疑问, java_home是否推荐设置?
export JAVA_HOME=`/usr/libexec/java_home -v 1.8`
#命令行指定某个java程序的执行jre
JAVA_HOME=`/usr/libexec/java_home -v 1.7` /your/program
#写个shell指定
#!/bin/sh
export JAVA_HOME=`/usr/libexec/java_home -v 1.7`
/your/program
```

此段落参考: http://www.kancloud.cn/kancloud/ocds-guide-to-setting-up-mac/71035

顺便说一句, 这个参考文档里面有很多不错的内容. 

```sh
#修改.bash_profile文件，添加如下内容：

export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_25.jdk/Contents/Home export 
export JAVA_7_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_71.jdk/Contents/Home export
alias jdk8='export JAVA_HOME=$JAVA_8_HOME'
alias jdk7='export JAVA_HOME=$JAVA_7_HOME'  
export JAVA_HOME=$JAVA_7_HOME
export PATH=$PATH:$JAVA_HOME/bin
#只需要在shell中输入jdk7或者jdk8就可以切换JDK版本了。
```

此代码参考: http://ju.outofmemory.cn/entry/114901

### 尾声2 activity的声明周期

发现一个问题, 还是使用了ondestroy, 在那里面我替换了这个activity的content, 是否这事引起了? 另外, onactivityresult里面也调用了super的, 不知是否有副作用.

### 尾声3 查看gradle的报错

1. Messages view可以看到, 据说有快捷键 ⌥0 不过, 我真的没有成功用出来这个快捷键.
2. message也是时有时没有的. 



然后, 聪明的读者一定发现, 我其实还没有解决问题, 续集在后文继续死磕: [2016-09-20-andorid四个bug和微信分享]