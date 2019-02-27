> 虽然之前看了不少内容, 但是, 最近艰难的再这个领域跋涉, 遇到很多问题, 进度太过喜人, 因此, 我们值得再次反思一下.

###### 参考mdn

- https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Editable_content
- 这里还提到了midas
- caret-color

- https://ckeditor.com/blog/ContentEditable-The-Good-the-Bad-and-the-Ugly/
- 这个是我主要参考的资料

###### 现状

- 市场上依旧没有统治级的编辑器. 没有好评爆表的编辑器. 那么, 我们做一个怎么样?
- 我们手上的材料:
  - contenteditable
    - 回车直接添加div太恶心了. 
    - 粘贴时自动style也太恶心了.
  - execCommand
    - selection, 如果这个考虑方向就更复杂了.
    - ranges
  - queryCommandState
  - mutation observers
    - 这个需要缓存选择 preserving selection
    - 或许还需要处理undo manager
  - 为了对抗inline style, 你不仅仅要处理粘贴, 回车…...
    - backspace
    - delete

###### 原文作者的结论

- contenteditable是魔鬼
- selection也是魔鬼
- 所以尽量少用他们
  - 自己建一套selection机制, 类似codemirror2/5, vscode这种.
  - 监听用户的输入, 监听键盘事件(其实需要一个input才行, 不然很难对付输入法)
  - 你要处理光标运动, 各种上下左右以及组合键和快捷键.
  - 你要自己处理粘贴, clipboard api要去研究下.
- 然后, 可以使用keyboardevent.key来处理用户输入.