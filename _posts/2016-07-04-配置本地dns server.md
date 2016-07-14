> 当我们, 需要在本地进行web测试时, 我们有各种方法. 改写配置文件host, 改.pac路由文件, 改路由器上面的设置. 但是, 这些方法都有一个问题, 就是要改回去的时候, 需要知道自己之前改了啥. 是否有一键解决方案呢? 有, 本地搭建一个dns server. 不用的时候停掉就好了.
>
> 另外, 还有一个问题, 各种测试框架的兼容性. 或者说各种工具的兼容性.

## dnsmasq

#### 安装dnsmasq

```sh
brew install dnsmasq
```

##### 安装结束看提示:

```sh
# To configure dnsmasq, copy the example configuration to /usr/local/etc/dnsmasq.conf.  and edit to taste.
cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf
# have launchd start dnsmasq now and restart at startup:
sudo brew services start dnsmasq
```

##### 如何修改配置文件呢? 这个要研究一下, 示例如下

```sh
---------------------------------
address=/ilanni.com/192.168.1.24 # 就这个 是必须的配置, 其他的都可以不配. 最好都别配, 因为我没验证过.
---------------------------------
address=/dev/127.0.0.1   # 这句活的意思是: 所有的.dev都转到127.0.0.1
resolv-file=/etc/resolv.dnsmasq.conf # 上游dns服务器地址指定. 有默认值, 可以不设置. 
strict-order # 严格解析顺序. 
listen-address=192.168.1.24, 127.0.0.1 # 监听地址, 可以逗号配置多个. 默认监听本机所有网卡.
server=112.124.47.27 #223.5.5.5 # 使用服务器解析, 这个服务器应该就是当前机器使用的dns了.
server=/google.com/8.8.8.8 #使用googledns解析google
bogus-nxdomain=223.5.5.5 #防止dns污染的设置. 如果是在阿里云服务器, 一定要开启这个项目.
```

##### 顺便说一下命令行打开文件.

```sh
open -a atom /usr/local/opt/dnsmasq/dnsmasq.conf.example  #这个意思是用atom打开.
open -a Sublime\ Text /usr/local/etc/dnsmasq.conf #打开配置文件
```

##### 需要把这个服务器在本机应用

```sh
# 其实不用那么复杂, 在mac的控制面板, 直接把dns指向127.0.0.1就好了. 
# 千万记得应用新的dns设置, 我就是忘记按应用按钮, 结果排查了半天.
sudo mkdir -p /etc/resolver
sudo tee /etc/resolver/dev >/dev/null <<EOF
nameserver 127.0.0.1
EOF
vi /etc/resolv.conf ，# 调整内容为 nameserver 192.168.1.78 
```

### 配置文件示例

```sh
# resolv-file=/etc/resolv.dnsmasq.conf
address=/test.hujing888.com/127.0.0.1
strict-order
listen-address=127.0.0.1, 192.168.1.106
server=112.124.47.27
```

### 测试

```sh
dig test.hujing888.com @127.0.0.1
ping test.hujing888.com
cat /etc/resolv.dnsmasq.conf 
cat /usr/local/etc/dnsmasq.conf
```

> 这个文档解释了, 如果只是解析某一类域名, 怎么办? https://passingcuriosity.com/2013/dnsmasq-dev-osx/
>
> 原文参考这个: http://tech.vg.no/2015/01/15/how-i-set-up-my-local-php-development-environment-on-mac-osx-yosemite-in-three-easy-steps/#postscriptum

### 只解析某些特定域名

- 文件配置 /etc/resolv.conf
- 目录要建在这里 /etc/resolver/
- 查看帮助: man 5 resolver

```sh
#原来这几句话的含义是这样的啊.
sudo tee /etc/resolver/dev >/dev/null <<EOF 
nameserver 127.0.0.1
EOF
```



## netbeans 支持php

> https://netbeans.org/kb/docs/php/configure-php-environment-mac-os.html

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

