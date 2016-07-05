> 当我们, 需要在本地进行web测试时, 我们有各种方法. 改写配置文件host, 改.pac路由文件, 改路由器上面的设置. 但是, 这些方法都有一个问题, 就是要改回去的时候, 需要知道自己之前改了啥. 是否有一键解决方案呢? 有, 本地搭建一个dns server. 不用的时候停掉就好了.
>
> 另外, 还有一个问题, 各种测试框架的兼容性. 或者说各种工具的兼容性.

#### 安装dnsmasq

```bash
brew install dnsmasq
```

#### 安装结束看提示: 

```sh
To configure dnsmasq, copy the example configuration to /usr/local/etc/dnsmasq.conf

and edit to taste.

  cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf

To have launchd start dnsmasq now and restart at startup:

  sudo brew services start dnsmasq

```

#### 如何修改配置文件呢? 

```sh
address=/dev/127.0.0.1   //这句活的意思是: 所有的.dev都转到127.0.0.1
```

#### 顺便说一下命令行打开文件.

```sh
open -a atom /usr/local/opt/dnsmasq/dnsmasq.conf.example  //这个意思是用atom打开.
```

#### 原文参考这个:

http://tech.vg.no/2015/01/15/how-i-set-up-my-local-php-development-environment-on-mac-osx-yosemite-in-three-easy-steps/#postscriptum

#### netbeans 支持php

https://netbeans.org/kb/docs/php/configure-php-environment-mac-os.html

mac + php , 如果我们脱离mamp定制一套—为了更大的自由度, 都需要啥?

1. zsh, 号称shell中的战斗机, 不是bash可以比的.
2. homebrew, 管理软件都用这个, 网上有各种优雅的安装软件的介绍.
3. composer, 据说是管理php包依赖的, 貌似也是必须装.
4. vagrant, 本地虚拟开发, 
   - 比如咱们可以模拟linux下面的开发— 原因是, 咱们的网站最终是部署到linux上面的. 
   - 他和虚拟机的区别是, 它比虚拟机轻多了.
   - 比如可以验证各种老版本的兼容性.
5. homestead, 虚拟机, 可以定制风格的.

> 参考: https://ericlbarnes.com/2015/08/04/set-mac-local-php-development/

#### 吐槽苹果

> 把ibook导入从itune挪到了ibook, 也不提示. 太奇怪了. 而且想不明白, 为啥废掉itune的统一管理的地位.

