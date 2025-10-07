> 语法高亮是编辑器的核心功能

```py
from PySide6.QtGui import QTextCursor, QTextCharFormat, QColor
from PySide6.QtWidgets import QApplication, QPlainTextEdit

app = QApplication([])
editor = QPlainTextEdit("hello world\nline two")
doc = editor.document() # QTextDocument

# 1. 定位到第 6-11 字符（world）
cursor = QTextCursor(doc)
cursor.setPosition(6)                  # 起点
cursor.setPosition(11, QTextCursor.KeepAnchor)  # 终点

# 2. 新建格式并染色
fmt = QTextCharFormat()
fmt.setForeground(QColor("#ff0"))
cursor.setCharFormat(fmt)              # 3. 应用

editor.show()
app.exec()
```



| 维度     | QTextEdit                      | QPlainTextEdit                | QSyntaxHighlighter                  | QTextCursor                          |
| -------- | ------------------------------ | ----------------------------- | ----------------------------------- | ------------------------------------ |
| **定位** | 富文本编辑器（HTML/表格/图片） | 纯文本高速编辑器（代码/日志） | **专给 PlainTextEdit 上色的外挂类** | **文本区间操作器**（选中/插入/染色） |
| **性能** | 慢（保留格式层）               | 快（无格式层，优化滚动）      | 只作用于 **PlainTextEdit**          | 任何 QTextDocument 都能用            |
| **用途** | 邮件、报表、Markdown           | IDE、终端、日志               | 代码高亮、行号、差分着色            | 行染色、括号匹配、自动补全           |

- **写代码** → **QPlainTextEdit + QSyntaxHighlighter**  `setDocument(plainTextEdit.document())`  
- **富文本** → **QTextEdit**  
- **染色/选区/插入** → **QTextCursor**（Highlighter 内部用，你也手动用）`setFormat()` 上色

```py
from PySide6.QtCore import Qt, QRegularExpression
from PySide6.QtGui import QColor, QFont, QTextCharFormat, QSyntaxHighlighter
from PySide6.QtWidgets import QApplication, QPlainTextEdit

app = QApplication([])

# 1. 编辑器
edit = QPlainTextEdit()
edit.setPlainText("hello world\n# 这是注释\nanother line")

# 2. 内联高亮器（无类文件）
class Highlighter(QSyntaxHighlighter):
    def __init__(self, parent=None):
        super().__init__(parent)
        fmt = QTextCharFormat()
        fmt.setForeground(QColor("#0f0"))
        fmt.setFont(QFont("Consolas", 14))   # ← 字号 14，PlainTextEdit 支持
        self._rule = (QRegularExpression(r"#.*"), fmt)

    def highlightBlock(self, text):
        for pattern, fmt in [self._rule]:
            match = pattern.match(text)
            if match.hasMatch():
                self.setFormat(match.capturedStart(), match.capturedLength(), fmt)

Highlighter(edit.document())

edit.show()
app.exec()
```

### 思路总结: 

1. webengine + js编辑器, 这个后患无穷不考虑, 因为, 万一有点问题, 就要深入js框架, 不论是codemirror/codeproser还是别的编辑器都不是十天半个月能搞定的.
2. 双document融合, 一个textedit拿用户输入, 然后, 格式化document之后在本textedit展示给用户???? 这个我不理解, 怎么做到的, 用户此时再输入不就乱套了????
   * 答: “双 Document 不是 把渲染结果塞回同一个 TextEdit
     * 用户永远只看见 A Document（只读、富文本、整页 HTML）
     * 键盘事件被 拦截→转 Markdown→改 A；UI 始终 WYSIWYG，不会乱
     * Typora 官方就用这招：底层两份 QTextDocument，一份 plain 做 diff，一份 html 给用户看
3. QQuickPaintedItem 或 ShaderEffect 自绘“块”, 数据层QAbstractListModel, 展示层QML的ListView { delegate: BlockDelegate {} } 这个其实有问题, qml不支持递归/迭代, 所以加载文档这一步就不知道怎么搞, 即便能搞也是后患无穷.
4. 在 scene 里自绘「块级 Item」, 看上去这个还是唯一方案....
   * 继承 QGraphicsItem（或 QFrame）写一系列子类
   * 每行输入完按 Enter → 用 markdown-parser 识别当前块类型 → 实例化对应 Item → 插入垂直布局；
   * 在 Item 内部直接用 QTextLayout/QSvgRenderer 把标题/代码/公式画出来；光标、选区、复制粘贴全部自己接管，形成“块级 WYSIWYG”。
      * 输入（键盘、IME、undo/redo）
      * 排版（按 Markdown 语法把块切成 HeadingItem、ParagraphItem、CodeBlockItem…
      * 渲染（用 QTextLayout / QSvgRenderer / MathJax-offline 小引擎 画出来）
      * 光标与选区（自己画 QRectF + 控制锚点）
      * 滚动（用 setPos() 整体平移，或把可见块放进 QGraphicsView::setSceneRect()）
   * QTextLayout/QTextBlock/QSvgRenderer 全套排版/绘图 API，官方 demo（Elastic Nodes、40000 Chips）已经证明 纯 item 自绘完全可行

* 问题来了, 据说typora就是用qt编写的, 那么他用的是什么技术路线呢?
  * 答: 方案2
