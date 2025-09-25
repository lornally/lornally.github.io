> 巨坑, 天坑.......

### property竟然一直变

```py

view = QGraphicsView()
d = {"a": 1}
view.setProperty("_test", d) # 这么搞一个property
print("首次 set 后 id:", id(view.property("_test")))
print("第一次 property 取:", id(view.property("_test")))
data = view.property("_test")  # 这一行是关键, 只要这个执行, 下面的id就会变
print("第二次 property 取:", id(view.property("_test")))
```

只要用view.property去给某个变量赋值, 那么view.property就变了... 这是什么rust特性...泰裤辣........  凸(艹皿艹 ) 

泰裤辣, (╯-_-)╯~╩╩  

这么一搞, 2-3个小时就过去了, 毫无意义的消耗生命ε(┬┬﹏┬┬)3

* grok有介绍: 

  * view.property("_test") 每次调用都会从 Qt 的 QVariantMap 创建一个新的 Python dict 对象，因此 ID 不同。直接在 print 中调用 id(view.property("_test")) 生成并丢弃一个新 dict，未存储。data = view.property("_test") 存储了新 dict，后续调用又生成另一个新 dict，导致 ID 变化。Qt 确保每次返回独立的 Python 对象，避免共享状态问题。

  * Qt 的 QObject.property() 行为在官方文档中说明，QVariant 到 Python 对象的转换会创建新对象，参见 Qt 文档（QVariant 和 QObject::property）。StackOverflow 上也有相关讨论，例如帖子提到 PyQt 属性访问创建新 dict 的行为（可搜索 "PyQt property QVariant dict id change"）。这是 Qt-Python 绑定机制的特性，确保数据隔离。

### 快捷键一样坑死

```py
# 替换快捷键为Cmd+Tab
# shortcut = QShortcut(QKeySequence(Qt.Modifier.META | Qt.Key.Key_Tab),                     tab)  # 这个不是推荐写法, 下面才是
shortcut = QShortcut(QKeySequence("Meta+Tab"), tab)  # Cmd+Tab

shortcut.activated.connect(lambda: tab.setCurrentIndex(
    (tab.currentIndex() + 1) % tab.count()))
# Cmd+W关闭标签页
close_shortcut = QShortcut(QKeySequence("Ctrl+W"), tab)
close_shortcut.activated.connect(lambda: tab.removeTab(tab.currentIndex()))
```

对, 你没看错, 写作meta, 实际是ctrl, 写作ctrl, 实际是cmd, 泰裤辣....

#### focus也能坑死

```py
class FocusTabWidget(QTabWidget):
    def focusInEvent(self, ev: QFocusEvent):
        # 把焦点直接塞给当前页里的 view
        view = self.currentWidget()
        if view and isinstance(view, QGraphicsView):
            view.setFocus(Qt.FocusReason.TabFocusReason)
        # 可选：让父类也处理，保持默认行为
        super().focusInEvent(ev)

tab = FocusTabWidget()
# 然而下面的代码并不生效
tab.setFocus(Qt.FocusReason.OtherFocusReason)
# 据说是因为此时并没有从别处转移焦点过来, 因此不触发focusevent


```

在Qt中，focusInEvent仅在控件实际接收焦点时（例如通过用户交互如点击、Tab键，或系统焦点转移）被调用，而setFocus只是请求将焦点设置到控件，但不保证触发focusInEvent，尤其在程序初始化时，焦点可能还未被系统分配。

```py
    # 因此新建view时, 设置焦点
    getab().setFocus() # 虽然没用, 但是写上吧....
    view.setFocusPolicy(Qt.FocusPolicy.StrongFocus)
    view.setFocus(Qt.FocusReason.TabFocusReason)
```





### 不完整代码

```py
# mini_tab_view.py
import sys

from PySide6.QtCore import Qt
from PySide6.QtGui import QFont, QKeySequence, QShortcut
from PySide6.QtWidgets import (QApplication, QGraphicsScene, QGraphicsTextItem,
                               QGraphicsView, QMainWindow, QTabWidget)

# ---------- 手工生命周期 ----------
app = QApplication(sys.argv)

# 1. 主窗口
win = QMainWindow()
win.setWindowTitle("最小 Tab+View")
win.resize(400, 300)

# 2. 一个 TabWidget
tab = QTabWidget()
win.setCentralWidget(tab)
tab.setTabsClosable(True)

# 3. 两个 Tab，每个 Tab 一个 Scene + View
name = "A.mm"
scene = QGraphicsScene()
scene.setBackgroundBrush(Qt.GlobalColor.darkGray)
item = QGraphicsTextItem(name)
item.setDefaultTextColor(Qt.GlobalColor.white)
item.setFont(QFont("", 14))
scene.addItem(item)

view = QGraphicsView(scene)
tab.addTab(view, name)

name = "B.mm"
scene2 = QGraphicsScene()
scene2.setBackgroundBrush(Qt.GlobalColor.darkGray)
item = QGraphicsTextItem(name)
item.setDefaultTextColor(Qt.GlobalColor.white)
item.setFont(QFont("", 14))
scene2.addItem(item)

view2 = QGraphicsView(scene2)
tab.addTab(view2, name)

# 焦点和事件设置
tab.setFocusPolicy(Qt.StrongFocus)
tab.setFocus()
win.setFocusProxy(tab)

# 替换快捷键为Cmd+Tab
shortcut = QShortcut(QKeySequence(Qt.Modifier.META | Qt.Key.Key_Tab),
                     tab)  # Cmd+Tab
shortcut.activated.connect(lambda: tab.setCurrentIndex(
    (tab.currentIndex() + 1) % tab.count()))
# Cmd+W关闭标签页
close_shortcut = QShortcut(QKeySequence("Ctrl+W"), tab)
close_shortcut.activated.connect(lambda: tab.removeTab(tab.currentIndex()))

win.show()
sys.exit(app.exec())

```

