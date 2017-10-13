> java的世界一直是人云亦云, 一帮傻子闭着眼睛说瞎话, 那么, 我们都不可信, 咋办? 笨办法, 一个一个试一下吧. 本文所有内容, 本人亲身尝试, 以身试毒, 神农再世啊.

### 编辑器咋办?

> 我只安装brew 可以安装的. https://blog.idrsolutions.com/2015/03/the-top-11-free-ide-for-java-coding-development-programming/
>
> github上面的列表: https://github.com/showcases/text-editors

这一波是java类.

1. enide studio 不可安装
2. bluej
   - Newbie users can check values and call methods on them, pass them as parameters and more and Java expressions can be invoked without compiling meaning BlueJ is a powerful graphical shell/REPL for Java.
   - there are popular textbooks designed for teaching introductory university/college courses with BlueJ, and a site full of teaching resources and is also can run on Windows, Mac OS X, Linux and other platforms which run Java. It can also run without installation from a USB stick.
3. jedit
   - Most people argue where jEdit beats many expensive development tools for features and ease of use is that the jEdit core comes with a built-in macro language; an extensible plugin architecture. Hundreds of macros and plugins are available.
4. jgrasp
   - jGRASP is a lightweight IDE primarily created for automatic generation of software visualizations to improve the comprehensibility of software. It is capable of producing static visualizations of source code structure and visualizations of data structures at runtime and jGRASP produces 
   - Control Structure Diagrams (CSDs) for Java, C, C++, Objective-C, Python, Ada, and VHDL; 
   - Complexity Profile Graphs (CPGs) for Java and Ada; UML class diagrams for Java; 
   - and has dynamic object viewers and a viewer canvas that work in conjunction with an integrated debugger and workbench for Java.
   - the viewers have a built in feature that allows it to identify data structures which allows it to recognize objects that represent traditional data structures such as stacks, queues, linked lists, binary trees, and hash tables and display this appropriately.
5. jsource 不可安装
6. jdeveloper 不可安装
7. drjava 不可安装

这一波是通用的

1. sublime text
   1. 利
      1. 插件成吨
      2. 美且好用
      3. 类似命令行的设置
      4. 多行编辑
      5. 非常快. 并且支持语言暴多.
   2. 缺点
      1. 加载时间比note++慢
      2. 和别的编辑器比, 这货的速度是优点, 不愧是基于emacs的编辑器, 就在windows上面慢, 所以他的语言也是lisp类的.
      3. 他的快捷键和mac一致, 是个巧合, 因为他是给予emacs的编辑器.
      4. 所以他不支持apple快捷键设置. 他是唯一的一个, 如果和atom, textmate放在一起的话.
2. vim, 这货都是mode, 一般我不用
3. atom, 
   1. 据说文件超过10m会崩溃.
   2. 据说比较耗内存.
   3. 插件列表: https://atom.io/packages 
   4. java插件: https://atom.io/users/noseglid 
   5. 他其实是一个定制的chrome, 所以插件都是js+css.
   6. 由此, 导致他的扩展性爆棚, 很容易变成ide.
   7. 目前看来是唯一支持apple快捷键设置(在控制面板中设置)的.
4. visual studio code
   1. 据说缺乏扩展, 但是本身支持很多了,所以问题不大.
   2. 据说升级困难.
5. notepad++
   1. 据说贼快
   2. 没有mac版本, 神奇了
6. brackets
   1. adobe: https://www.upwork.com/hiring/development/text-editors-atom-sublime-brackets/
   2. 看上去不错
   3. 牛死了, 带自动切图功能, 可以自动从photoshop拿到颜色, 宽度, 等等.
   4. 其实他是一个前端编辑器.
7. [Gedit](https://wiki.gnome.org/Apps/Gedit)
8. [TextPad](https://www.textpad.com/)
9. [Geany](https://www.geany.org/)
   1. 参考: https://www.toptal.com/it/programming-editors-a-never-ending-battle-with-no-clear-winner
   2. 快, 风格看上去很老式.
10. [Komodo Edit](http://komodoide.com/komodo-edit/)
  1. 貌似是个比较轻量的ide?
11. [Light Table](http://lighttable.com/)
12. libreoffice
    1. 就是个office
13. coda
    1. 这个看上去貌似靠谱: https://www.codetuts.tech/text-editor-or-ide-to-code/
    2. 像个微型ide.

###### app段落总结

> http://www.makeuseof.com/tag/best-mac-os-x-apps/#photos

### textmate

```sh
 # 格式化代码
   Command + Option + [
   Ctrl+Q
   Command + Shift + {
 # 插件装上emmet
```
利

- 支持apple全局快捷键设置.
- 快.
- 原生在mac, 所有mac原生支持内容, 他都支持.
- 够老, 因此各种插件齐全, sublime和atom上面的很多插件都是从这里移植过去的.
- object-c原生mac代码.


弊

- 社区不够活跃.
- '老'编辑器, 不如新生的编辑器火爆.

> 选他是有原因的, 
>
> 1. 他对mac原生的支持(比如快捷键)是最好的, 当然他是xcode+object-c搞得, 和atom(给予chrome, chrome又给予blink, blink是webkit的一个分支, webkit是safari和mail的内核)一样好.
> 2. 他的速度快, 和sublime一样快, atom给予浏览器, 因此atom的速度和内存都有点问题, 打开大文件会出事. 当然这也是google在教育我们 - 代码文件就不要太大.
> 3. 简单地说, sublime和atom各自有个小短板, 确实问题也不大, 但是实际使用中还是挺烦人的, 能逼死我这样的强迫症.