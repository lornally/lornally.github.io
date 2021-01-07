> 苹果的各种link, Symlink, Alias, Shortcut softlink, hardlink, firmlink

- alias 古老, 强大, 方便, 他实际上是一个小的描述文件
  - 强大之处: 包含源文件的inode name
    - 即便源文件移动, alias也不受影响
    - alias建立之后, 本身也可以随意移动
  - 建立方式很多, 认准: make alias
  - 右键
    - file菜单
    - 工具栏的...
    - alt+cmd+drag
  - 弱点: 这是一个finder功能, 几乎没有兼容性可言, 命令行和其他工具都无法使用这个.
- symbolic link 就是softlink
  - unix的规范, 也是也给描述文件
  - 但是不包含inode name, 因此, 不能瞎移动
  - 但是这也可以说是一个优点, 也就是说只认文件路径, 那么我们可以在原始位置对文件进行替换.
  - 有开源项目可以右键新建https://superuser.com/questions/24095/create-a-symbolic-link-in-the-mac-os-x-finder
  - 用appscript似乎更合理https://apple.stackexchange.com/questions/357022/quick-way-to-create-a-symlink

```sh
ln -s /path/to/file /path/to/link
```

- hardlink
  - 也是unix规范, 也是描述文件
  - 包含inode name, 但是不包含pathname
  - 要删除原始文件, 必须先删除所有的hardlink
  - 给命令行建hardlink Amit Singh's six rules: 
    - The file system must be journaled HFS+. 
    - The parent directories of the source and destination must be different. 
    - The source's parent must not be the root directory. 
    - The destination must not be in the root directory.
    -  Hard links All three types of links are shortcuts to an original file system object. 
    - A file system object is usually a file on your Mac, but it can also be a folder, a drive, or a networked device.

```sh
ln /path/to/file /path/to/hardlink
# 如果命令行需要 -d或者-F参数
```

- firmlink是苹果从10.15引入的特性. 貌似是hard版的alias, 
  - 他是双向的. 
  - https://arstechnica.com/gadgets/2019/10/macos-10-15-catalina-the-ars-technica-review/11/
  - 但是, 目前只能系统使用, 作为user, 没找到直接使用的办法.

###### 辅助操作

```sh
ls -lt # 查看是否link
```



###### 命令行读取alias

- 大神的代码解析; https://eclecticlight.co/2018/03/16/accessing-finder-aliases-in-your-own-code-a-walk-through-alismas-source/