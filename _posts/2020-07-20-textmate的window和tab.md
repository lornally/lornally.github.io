> 偶然发现textmate有一个神奇的产品逻辑: 什么时候用tab, 什么时候用window?

- http://textmate.1073791.n5.nabble.com/open-file-NOT-into-tab-of-existing-window-td31816.html

作者的解释:

- 当我们任意打开一个文件的时候, textmate就认为打开了一个project. 
- 如果第二个文件打开的位置是这个project所在的目录, 或者子目录, 那么就是tab打开. 
- 如果强制window打开, 那么请按option键.

这里其实是对一个重大问题的一次尝试: 打开一个文件, 用tab还是window

- sublime/vscode/bear都是用tab的. 所有情况都用tab.
  - 但是sublime/vscode提供了分屏功能, 用起来还算方便.(注意: 他们并不自动形成项目, 项目目录要手动拖进去)
  - bear实际上复制了文档副本到了他自己的目录结构里面.
- typora/空格预览/苹果一般的软件逻辑都是直接用一个window.
  - 但是苹果软件全屏时有一个选项使用tab. : 打开文稿时首选标签页: 仅在全屏/始终/手动.
  - typora是左边可以切换大纲和目录. 可以直接切换文档, 并不会形成tab. 这个解决方案也不错.
- textmate在这里用了一个比较纠结的解决方案. 
  - 自动形成project, 类似typora.
  - 同project用tab. 不同的project用window.
  - 强制window打开, 需用快捷键option.