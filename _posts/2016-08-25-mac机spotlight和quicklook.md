> 这是两个基础功能, 但是一堆坑.

## 问题

- spotlight, 不能索引某些文本文件, 比如: markdown. 
- quicklook, 不能看某些文件, 比如某些文本文件和mindmap(比如freemind的mm).

## 索引md

mac 索引 md , 可以安装qlmarkdown, 请参考: 

- [https://github.com/toland/qlmarkdown]
- [http://stackoverflow.com/questions/365669/how-can-i-make-spotlight-index-markdown-files]
- 然后发现貌似不起作用, 是否要重建索引?
  - [https://support.apple.com/en-us/HT201716]
  - 也可以命令行重建索引.
- cool 成功了.


还有另外一条路, 使用richtext的plist:

- [http://hiltmon.com/blog/2015/11/17/a-yosemite-markdown-spotlight-importer/]

- [http://brettterpstra.com/2011/10/18/fixing-spotlight-indexing-of-markdown-content/]

## mindmap(freemind)的quicklook

- 重新安装freemind, 但是, 依旧不行.

- 不搞了, 基本就是不行.

- 历史上成功过, 但是, 目前是不行的.

  ```sh
   qlmanage -p qulitest.mm #用这行代码就可以看到问题.
  ```

  ​
