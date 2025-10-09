> pyside的各种.... 太方便了

### 举例

- 想拖动画布？`setDragMode(ScrollHandDrag)` 一行搞定  
- 想框选 item？`setDragMode(RubberBandDrag)` 也是一行  
- 想缩放？`wheelEvent` 里 `scale()` 一下就行  
- 想捏合？`grabGesture(Qt.PinchGesture)` 就能收到原生手势  
- item的setTextInteractionFlags

### 列表

---

### 一、View 级（QGraphicsView）  
“画布”本身的行为，缩放/平移/手势/对齐一股脑都在这里。

| 目标         | 一行代码                                                   | 说明               |
| ------------ | ---------------------------------------------------------- | ------------------ |
| 左键拖拽平移 | `view.setDragMode(QGraphicsView.ScrollHandDrag)`           | 左键拖 = 移动场景  |
| 框选 item    | `view.setDragMode(QGraphicsView.RubberBandDrag)`           | 左键拖 = 拉橡皮筋  |
| 滚轮缩放     | 见下方 `wheelEvent` 模板                                   | 以光标为中心       |
| 两指捏合     | `view.grabGesture(Qt.PinchGesture)` + `event.gesture(...)` | 完整代码在隔壁鼠标 |
| 抗锯齿       | `view.setRenderHint(QPainter.Antialiasing)`                |                    |
| 对齐网格     | `view.setAlignment(Qt.AlignLeft \| Qt.AlignTop)`           |                    |
| 隐藏滚动条   | `view.setHorizontalScrollBarPolicy(Qt.ScrollBarAlwaysOff)` |                    |

---

### 二、Scene 级（QGraphicsScene）  
“模型”层，只管逻辑坐标、item 检索、事件分发。

| 目标         | 一行代码                                                     | 说明                             |
| ------------ | ------------------------------------------------------------ | -------------------------------- |
| 场景矩形     | `scene.setSceneRect(-1000, -1000, 2000, 2000)`               | 无限大默认                       |
| 背景网格     | `scene.setBackgroundBrush(QBrush(Qt.gray, Qt.CrossPattern))` |                                  |
| 点击选不中？ | `scene.itemIndexMethod()` 默认就是 BspTreeIndex，一般不动    |                                  |
| 批量碰撞     | `items = scene.items(QPointF(x, y))`                         | 返回列表，可过滤 type            |
| 视图同步     | `view.setScene(scene)`                                       | 一对多也允许多个 view 挂同一场景 |

---

### 三、Item 级（QGraphicsItem 家族）  
真正“会动”的东西，OO 精华最密集。

#### 1. 文本/编辑相关
| 目标     | 一行代码                                                 | 说明                        |
| -------- | -------------------------------------------------------- | --------------------------- |
| 文本可选 | `item.setTextInteractionFlags(Qt.TextSelectableByMouse)` | 对 `QGraphicsTextItem` 有效 |
| 文本可改 | `item.setTextInteractionFlags(Qt.TextEditable)`          |                             |
| 链接可点 | `item.setOpenExternalLinks(True)`                        | 同样只对 TextItem           |
| 字体     | `item.setFont(QFont("Arial", 12))`                       |                             |
| 换行宽度 | `item.setTextWidth(200)`                                 | 0=不自动换行                |

#### 2. 标志位（QGraphicsItem.GraphicsItemFlag）
| 目标     | 一行代码                                                 | 说明                     |
| -------- | -------------------------------------------------------- | ------------------------ |
| 可移动   | `item.setFlag(QGraphicsItem.ItemIsMovable)`              |                          |
| 可聚焦   | `item.setFlag(QGraphicsItem.ItemIsFocusable)`            | 否则收不到键盘事件       |
| 可选取   | `item.setFlag(QGraphicsItem.ItemIsSelectable)`           | 和 RubberBandDrag 联动   |
| 不画边框 | `item.setFlag(QGraphicsItem.ItemClipsToShape)`           |                          |
| 忽略缩放 | `item.setFlag(QGraphicsItem.ItemIgnoresTransformations)` | 文字标签固定像素大小神器 |
| 缓存加速 | `item.setCacheMode(QGraphicsItem.DeviceCoordinateCache)` |                          |

#### 3. 坐标/变形
| 目标         | 一行代码                                                     | 说明                                   |
| ------------ | ------------------------------------------------------------ | -------------------------------------- |
| 转场景坐标   | `pt_scene = item.mapToScene(item.rect().topLeft())`          |                                        |
| 转视图坐标   | `pt_view = view.mapFromScene(pt_scene)`                      |                                        |
| 自身旋转     | `item.setRotation(45)`                                       |                                        |
| 自身缩放     | `item.setScale(1.5)`                                         | 不影响子级就用 `setTransform()` 更灵活 |
| 原点移到中心 | `item.setTransformOriginPoint(item.boundingRect().center())` |                                        |

---

### 四、速用模板（无 class 版）
把下面三坨黏一起就是一个“可滚轮+捏合+拖拽 item”的最小画布：

```python
import sys
from PySide6.QtCore import *
from PySide6.QtWidgets import *
from PySide6.QtGui import *

app = QApplication(sys.argv)

# 1. 场景
scene = QGraphicsScene(-500, -500, 1000, 1000)

# 2. 视图
view = QGraphicsView(scene)
view.setDragMode(QGraphicsView.ScrollHandDrag)
view.grabGesture(Qt.PinchGesture)
view.resize(800, 600)

# 3. 随手放两个 item
text = scene.addText("PySide6\n可编辑文字", QFont("Arial", 20))
text.setTextInteractionFlags(Qt.TextEditorInteraction)   # ← 精华一行
rect = scene.addRect(QRectF(-60, -60, 120, 120),
                     QPen(Qt.black), QBrush(Qt.green))
rect.setFlag(QGraphicsItem.ItemIsMovable)                # ← 精华一行
rect.setFlag(QGraphicsItem.ItemIsSelectable)

# 4. 缩放函数（滚轮 & 捏合）
def zoom_at(pos, factor):
    anchor = view.mapToScene(pos.toPoint())
    view.scale(factor, factor)
    delta = view.mapToScene(pos.toPoint()) - anchor
    view.translate(delta.x(), delta.y())

view.wheelEvent = lambda ev: (zoom_at(ev.position(), 1.15 if ev.angleDelta().y()>0 else 0.87),
                              ev.accept())
view.event = lambda ev: (zoom_at(ev.gesture(Qt.PinchGesture).centerPoint().toPoint(),
                                 ev.gesture(Qt.PinchGesture).scaleFactor())
                         if ev.type()==QEvent.Gesture and ev.gesture(Qt.PinchGesture)
                         else QGraphicsView.event(view, ev))

view.show()
sys.exit(app.exec())
```

---

### 扩展内容:
- QGraphicsItem 做 自定义形状 + 碰撞检测 → 必须重载 `boundingRect()` / `paint()` / `shape()`  
- 动画/帧率  → QTimeLine, QPropertyAnimation  
- OpenGL 加速 → `QOpenGLWidget` 作为 viewport  
- 多线程场景渲染 → QGraphicsSceneBspTreeIndex 调优参数  

