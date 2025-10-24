> view就是widget

### 关系

view/scene/item架构中view有两个:

* 父类: QGraphicsView, 基础类, 一般情况都用它
* 子类: QChartView, 用来展示图表, 配合QChart使用, 
  * QChart 继承自 `QGraphicsWidget`，而 `QGraphicsWidget` 又继承自 `QGraphicsObject` → 它是 scene 里的一棵 item 树
  * 不论是qchartview还是qchart都不是qt核心库成员, 而是qtchart库成员
  * 类似的还有qtsvg的QGraphicsSvgItem

### 内容

一、setWindowFlags 常用值（**对系统窗口管理器说话**）

| 标志                       | 作用                             |
| -------------------------- | -------------------------------- |
| `Qt::FramelessWindowHint`  | 去掉系统标题栏/边框              |
| `Qt::WindowStaysOnTopHint` | 本应用内最上层                   |
| `Qt::Tool`                 | 小工具板（紧凑标题、不进 Cmd+`） |
| `Qt::Window`               | 普通独立窗口（默认就有）         |
| `Qt::Popup`                | 弹出层（点击外部自动消失）       |
| `Qt::Dialog`               | 对话框样式（仅关闭按钮）         |

**用法**：按位或  
```python
view.setWindowFlags(Qt.Tool | Qt.FramelessWindowHint)
```

------------------------------------------------
二、setAttribute 常用值（**对 Qt 自身行为说话**）

| 属性                       | 作用                                                |
| -------------------------- | --------------------------------------------------- |
| `WA_TranslucentBackground` | 允许背景透明（你自己画 rgba）                       |
| `WA_DeleteOnClose`         | 关闭时自动 `delete`                                 |
| `WA_ShowWithoutActivating` | 弹出但不抢焦点（适合浮层提示）                      |
| `WA_NoSystemBackground`    | 不擦除背景（省一次 fillRect）                       |
| `WA_AlwaysShowToolTips`    | 父窗未激活也显示 tooltip                            |
| `WA_Hover`                 | 强制产生 `hoverMoveEvent`（无此则只有 enter/leave） |
| WA_NoMousePropagation      | 不接受焦点                                          |

**用法**：单属性开关  
```python
view.setAttribute(Qt.WA_TranslucentBackground, True)
```

------------------------------------------------
三、view 的「拖动/缩放/手势」设置 —— **都不是虚函数**，只是普通接口

| 功能     | 接口（非虚函数）                                             | 说明                      |
| -------- | ------------------------------------------------------------ | ------------------------- |
| 滚轮缩放 | `view.setTransformationAnchor(QGraphicsView.AnchorUnderMouse)` | 锚点                      |
| 居中缩放 | `view.setResizeAnchor(...)`                                  | 窗口resize时锚点          |
| 抗锯齿   | `view.setRenderHint(QPainter.Antialiasing)`                  | 普通设置                  |
| 拖拽模式 | `view.setDragMode(QGraphicsView.ScrollHandDrag)`             | 手型拖画布                |
| 手势     | `view.grabGesture(Qt.PinchGesture)`                          | 注册即可收 `gestureEvent` |
| 滚动条   | `view.setVerticalScrollBarPolicy(Qt.ScrollBarAlwaysOff)`     | 关掉原生滚动条            |
| 对齐网格 | `view.setAlignment(Qt.AlignLeft | Qt.AlignTop)`              | 视图对齐方式              |

------------------------------------------------
**QGraphicsView 就是 Qt 的“输入中枢”**——**所有鼠标、滚轮、键盘、手势、触摸、拖拽、快捷键、缩放、平移**都能通过它捕获

------------------------------------------------
一、鼠标 & 滚轮（最常用）

| 功能         | 相关接口/设置                      | 典型一行代码                |
| ------------ | ---------------------------------- | --------------------------- |
| 左键点击拾取 | 重载 `mousePressEvent`             | `it = self.itemAt(e.pos())` |
| 中键拖拽平移 | `setDragMode(ScrollHandDrag)`      | 一行设置，自带手型          |
| 滚轮缩放     | 重载 `wheelEvent` + `scale()`      | `self.scale(1.15, 1.15)`    |
| 鼠标样式     | `setCursor(Qt::OpenHand)`          | 实时变手型                  |
| 拾取精度     | `setViewportUpdate(MinimalUpdate)` | 减少闪烁                    |

------------------------------------------------
二、键盘 & 快捷键

| 功能             | 相关接口/设置                    | 典型一行代码                                                 |
| ---------------- | -------------------------------- | ------------------------------------------------------------ |
| 方向键移动选中项 | 重载 `keyPressEvent`             | `if e.key()==Qt.Key_Left: item.moveBy(-10,0)`                |
| 全局快捷键       | `QAction + QKeySequence`         | `action = QAction("Del", self); action.setShortcut(QKeySequence.Delete)` |
| 视图级热键       | `QShortcut(self)`                | `QShortcut(QKeySequence("Ctrl+="), self, self.zoomIn)`       |
| 焦点策略         | `setFocusPolicy(Qt.StrongFocus)` | 让 View 能收键盘事件                                         |

------------------------------------------------
三、手势 & 多点触控（macOS trackpad / Win 触控）

| 功能         | 相关接口/设置                  | 典型一行代码                       |
| ------------ | ------------------------------ | ---------------------------------- |
| 两指捏合缩放 | `grabGesture(Qt.PinchGesture)` | 收 `QPinchGesture`                 |
| 两指滑动平移 | `grabGesture(Qt.PanGesture)`   | 收 `QPanGesture`                   |
| 兜底总入口   | 重载 `event()`                 | `if e.type()==QEvent.Gesture: ...` |

------------------------------------------------
四、拖拽（文件拖进来生成节点）

| 功能         | 相关接口/设置          | 典型一行代码                   |
| ------------ | ---------------------- | ------------------------------ |
| 接收文件拖放 | `setAcceptDrops(True)` | View 级                        |
| 开始拖拽     | `QDrag + mimeData`     | Item 里 `mousePressEvent` 启动 |
| 拖拽进入     | 重载 `dragEnterEvent`  | 判断 `mimeData().hasUrls()`    |
| 放下         | 重载 `dropEvent`       | `pos = event.scenePos()`       |

------------------------------------------------
五、缩放/旋转/对齐（非虚函数，纯设置）

| 功能             | 相关接口/设置                               | 典型一行代码      |
| ---------------- | ------------------------------------------- | ----------------- |
| 以光标为中心缩放 | `setTransformationAnchor(AnchorUnderMouse)` | 单手型缩放        |
| 平滑缩放         | `setRenderHint(Antialiasing)`               | 边缘不锯齿        |
| 旋转             | `rotate(angle)` / `setTransform`            | `view.rotate(15)` |
| 对齐网格         | `setAlignment(Qt.AlignLeft | Qt.AlignTop)`  | 视图左上角对齐    |
| 最大性能         | `setViewportUpdate(MinimalUpdate)`          | 只重绘变化矩形    |

------------------------------------------------
六、触摸 & 平板笔

| 功能       | 相关接口/设置                                                | 典型一行代码                 |
| ---------- | ------------------------------------------------------------ | ---------------------------- |
| 多点触控   | `grabGesture(Qt.TapGesture | Qt.PanGesture | Qt.PinchGesture)` | 统一收 `QGesture`            |
| 压感/倾斜  | 重载 `tabletEvent(QTabletEvent)`                             | 收 `pressure/rotation`       |
| 兜底总入口 | 重载 `event()`                                               | 先拦截 `QEvent::TouchUpdate` |

------------------------------------------------
七、事件过滤 & 全局钩子

| 功能           | 相关接口/设置                   | 典型一行代码           |
| -------------- | ------------------------------- | ---------------------- |
| 全局事件过滤器 | `installEventFilter(self)`      | 可拦截所有事件         |
| Scene 级过滤器 | `installSceneEventFilter(item)` | 只过滤指定 Item 的事件 |
| 底层兜底       | 重载 `event(QEvent)`            | 任何新事件都能先拦截   |

------------------------------------------------
八、性能 & 调试开关

| 功能         | 相关接口/设置                                      | 典型一行代码     |
| ------------ | -------------------------------------------------- | ---------------- |
| 显示 FPS     | `setViewport(new QOpenGLWidget)` + `QElapsedTimer` | 60 FPS 实测      |
| 只更新变化区 | `setViewportUpdate(MinimalUpdate)`                 | 大场景不闪       |
| 打印场景     | `render(QPainter, targetRect, sourceRect)`         | 一键导出 PNG/PDF |

------------------------------------------------
#### 处理分类

* 手动处理事件(大部分情况并不需要), 重载某个虚函数xxxEvent
* 缩放平移 → setXxx + 非虚函数
* 手势触摸 → grabGesture + event() 兜底
* 性能 → OpenGL + MinimalUpdate

### 关于虚函数和设置

* setXxx / grabXxx / scale / rotate → 普通接口，非虚函数，按需开关
* xxxEvent → 虚函数，以 Event 结尾，必须重载才能收事件

所以：

- 想「打开平滑缩放」→ 调 `setRenderHint()`（**普通接口**）
- 想「自己处理滚轮」→ 重载 `wheelEvent()`（**虚函数**）

### 「**用普通接口 vs 必须虚函数**」的**红绿灯表

------------------------------------------------
🟢 普通接口就够（**不调也能跑**）

| 功能                  | 接口/设置                                                    | 是否必须自己写代码                    |
| --------------------- | ------------------------------------------------------------ | ------------------------------------- |
| 无父全局菜单 + 快捷键 | `QAction::setShortcut`                                       | **零代码**，菜单栏挂 action 即可      |
| 画布整体拖动          | `view.setDragMode(ScrollHandDrag)`                           | **一行设置**，自带手型                |
| 双指滑动平移          | `view.grabGesture(Qt.PanGesture)`                            | **一行注册**，收 `QPanGesture` 即可   |
| 双指捏合缩放          | `view.grabGesture(Qt.PinchGesture)`                          | **一行注册**，收 `QPinchGesture` 即可 |
| 滚轮缩放              | `view.wheelEvent` 里 `scale()`                               | 可选重载，**默认也能缩放**            |
| 右键菜单(一致的)      | # 1. 让视图能发右键信号<br />self.setContextMenuPolicy(Qt.CustomContextMenu)<br /># 2. 连接信号 → 普通槽函数（非虚函数）<br />self.customContextMenuRequested.connect(菜单函数下面是2行核心代码)<br />menu = QMenu()<br />menu.exec(self.mapToGlobal(pos)) |                                       |

------------------------------------------------
🔴 必须虚函数（**你不写，Qt 不会帮你**）

| 功能                          | 虚函数                                          | 原因                                            |
| ----------------------------- | ----------------------------------------------- | ----------------------------------------------- |
| **空白 vs Item 不同右键菜单** | `Scene::contextMenuEvent`                       | 系统只分发一次，**你必须 hitTest 后决定给谁弹** |
| **Item 键盘+鼠标组合**        | `Item::keyPressEvent` + `Item::mousePressEvent` | **Item 才收得到**，Scene/View 不会自动转发      |
| **手势兜底/新事件**           | `View::event()`                                 | 只有这里能先拦截**未公开**事件                  |

------------------------------------------------
🟡 可选虚函数（**普通接口也能跑，但重载更精细**）

| 功能             | 普通接口                  | 重载好处               |
| ---------------- | ------------------------- | ---------------------- |
| 滚轮缩放         | `wheelEvent` 里 `scale()` | 可控制倍率、锚点、动画 |
| 画布边界自动扩大 | `scene.setSceneRect`      | 可动态计算，不用每帧调 |

------------------------------------------------
结论

> **「全局菜单、整体拖动、双指滑动/捏合、滚轮缩放」** → **普通接口一行搞定**  
> **「空白/Item 不同菜单、Item 组合键、新手势兜底」** → **必须写虚函数**  

- **普通接口**负责「**整体行为**」  
- **虚函数**只负责「**命中测试 + Item 私有交互**」  

