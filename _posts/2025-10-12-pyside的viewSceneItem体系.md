| 动机             | 传统 QWidget 的问题                | View-Scene-Item 的解决方案                              |
| ---------------- | ---------------------------------- | ------------------------------------------------------- |
| **性能**         | 每个控件都是系统窗口，几千个就卡爆 | Item 是轻量对象，无系统窗口，Scene 统一渲染             |
| **坐标系统混乱** | 控件坐标系固定，缩放/旋转麻烦      | Scene 提供逻辑坐标系，View 负责映射到屏幕               |
| **交互逻辑混乱** | 事件直接到控件，难以统一管理       | 事件由 View → Scene → Item 分发，可拦截、可代理、可扩展 |

| 层级      | 职责                                             | 类比           |
| --------- | ------------------------------------------------ | -------------- |
| **Item**  | 图形对象本身（形状、行为、数据）                 | “画布上的图形” |
| **Scene** | 管理所有 Item 的生命周期、事件、碰撞、索引       | “画布”         |
| **View**  | 控制如何“看”这个画布（缩放、旋转、滚动、多视图） | “摄像机”       |

- 在一个 **Scene** 里放 10 万个粒子；
- 用 **View1** 放大看局部，用 **View2** 看全局缩略图；
- 每个粒子是一个 **Item**，可以响应点击、拖拽、动画；
- 所有事件由 Scene 统一分发，View 只负责“怎么看”。

### view -> scene -> item是一个事件分发体系

* **若任何一层调用 `event->accept()` 或不调 `super().xxxEvent(e)`，链即中断，下层再也收不到**

### 例如：

- View 上重写了 `mousePressEvent`
- Scene 上重写了 `mousePressEvent`
- Item 上重写了 `mousePressEvent`

然后点击了那个 Item：

#### 行为：

1. **View 的 `mousePressEvent` 被调用**（系统事件入口）
2. 如果你不调用 `super().mousePressEvent(event)`，**事件链在这里断了**，Scene 和 Item 都收不到。
3. 如果你调用了 `super().mousePressEvent(event)`，View 会把事件发给 Scene。
4. **Scene 的 `mousePressEvent` 被调用**, 同样需要super().mousePressEvent()才能把事件转发给item
5. 如果此时, 不用class写法, 而是scene.contextMenuEvent = lambda e: ..., 那么就覆盖了虚函数表, 此时无论如何也不会把事件转发给item了.



### 继承关系

```py
QObject
├─ QWidget                                    # 窗口/控件基类
│  ├─ QGraphicsView                         # 场景窗口（也是 QWidget）
│  └─ 其他所有传统控件（QPushButton...）
└─ QGraphicsScene  ←–––––––––––––  QObject      # 场景（非 QWidget，是 QObject）

QGraphicsItem                                 # 纯 C++ 轻量接口（非 QObject）
├─ QGraphicsPathItem                         # 路径
├─ QGraphicsRectItem                         # 矩形
├─ QGraphicsEllipseItem                      # 椭圆
├─ QGraphicsPolygonItem                      # 多边形
├─ QGraphicsLineItem                         # 线段
├─ QGraphicsPixmapItem                       # 位图
├─ QGraphicsTextItem  （也可直接继承自 QGraphicsObject） 
└─ QGraphicsSimpleTextItem                   # 轻量文字（非 QObject）

QGraphicsItem                                 # 纯 C++ 轻量接口（非 QObject）
├─ QGraphicsObject  ←–––––––  QObject        # 带信号槽的 Item
│  ├─ QGraphicsWidget                        # 可布局的 Item 版“Widget”
QGraphicsLayoutItem                          # 可以被布局系统管理
├─ QGraphicsWidget                           # 多重继承了QGraphicsObject
│  └─ QGraphicsProxyWidget                # 把真实 QWidget 嵌进场景
└─ QGraphicsLayout
   ├─ QGraphicsLinearLayout
   └─ QGraphicsGridLayout


QPen        ←  QPaintEngine 数据类           # 画笔（非 QObject）
QBrush      ←  QPaintEngine 数据类           # 画刷（非 QObject）
QPainterPath ← 纯数据类                      # 路径（非 QObject）
```

* QGraphicsWidget  ←––– 同时继承下面两项: 
  1. QGraphicsObject   (提供 QObject 信号槽)
  2. QGraphicsLayoutItem  (能被 QGraphicsLinearLayout 管理)
* **QGraphicsWidget** 是 **唯一既能被 `QGraphicsLinearLayout` 管理、又能管理 `QGraphicsLayout` 的类**。
* 普通 `QGraphicsItem`（矩形、椭圆…）**不是** `QGraphicsLayoutItem`，**不能** 直接放进 `QGraphicsLinearLayout`；要想让它们享受布局，只能包一层 `QGraphicsWidget` 当容器；

### 事件处理原则

1. 能用虚函数尽量用虚函数
2. 不得已才在event里面兜底
3. 虚函数都是Event结尾的

### view

| 类别 | 虚函数| 典型用途|
| ---- | --------------- | ---------- |
| 鼠标 | `mousePressEvent` / `mouseMoveEvent` / `mouseReleaseEvent` / `mouseDoubleClickEvent` | 拖拽、框选、画图        |
| 滚轮 | `wheelEvent`                                                 | 缩放                    |
| 键盘 | `keyPressEvent` / `keyReleaseEvent`                          | 快捷键、Del 删图元      |
| 菜单 | `contextMenuEvent`                                           | 右键菜单                |
| 拖放 | `dragEnterEvent` / `dragMoveEvent` / `dropEvent` / `dragLeaveEvent` | 文件拖进来创建节点      |
| 手势 | `gestureEvent`                                               | 捏合、滑动              |
| 触摸 | `touchEvent`                                                 | 多点触控                |
| 焦点 | `focusInEvent` / `focusOutEvent`                             | 很少用，Item 焦点更常用 |
| 大小 | `resizeEvent`                                                | 窗口变尺寸时重新布局    |
| 绘制 | `drawBackground` / `drawForeground`                          | 自定义网格、水印        |
| 总线 | `event`                                                      | 兜底过滤所有事件        |

### scene

| 类别     | 虚函数签名                                                   | 触发时机                     |
| -------- | ------------------------------------------------------------ | ---------------------------- |
| 鼠标     | `mousePressEvent` / `mouseMoveEvent` / `mouseReleaseEvent` / `mouseDoubleClickEvent` | 任意键按下、移动、释放、双击 |
| 鼠标     | `wheelEvent`                                                 | 滚轮                         |
| 菜单     | `contextMenuEvent`                                           | 右键（或菜单键）             |
| 键盘     | `keyPressEvent` / `keyReleaseEvent`                          | 获得焦点后键盘按键           |
| 拖拽     | `dragEnterEvent` / `dragMoveEvent` / `dropEvent` / `dragLeaveEvent` | 拖放                         |
| 焦点     | `focusInEvent` / `focusOutEvent`                             | 场景整体获得/失去焦点        |
| 手势     | `gestureEvent`                                               | 系统手势（捏合、滑动等）     |
| 触摸     | `touchEvent`                                                 | 触摸屏多点序列               |
| 事件总线 | `event`                                                      | 所有事件兜底                 |

### item

有，而且数量比 View/Scene 还多——`QGraphicsItem` 是 **事件链最末端**，所有“原子行为”都在这里给一次**最终拦截**机会。  
按用途分组，常用虚函数如下（PySide 写法）：

------------------------------------------------
一、几何 & 绘制
```python
boundingRect()              # 必须实现
paint(QPainter, …)          # 必须实现
shape()                     # 可选，精确碰撞轮廓
isObscuredBy() / opaqueArea()  # 深度优化才用
```

------------------------------------------------
二、交互事件（与 View/Scene 同名，但参数是 `QGraphicsSceneEvent` 子类）
```python
mousePressEvent(QGraphicsSceneMouseEvent)
mouseMoveEvent(QGraphicsSceneMouseEvent)
mouseReleaseEvent(QGraphicsSceneMouseEvent)
mouseDoubleClickEvent(QGraphicsSceneMouseEvent)
wheelEvent(QGraphicsSceneWheelEvent)
contextMenuEvent(QGraphicsSceneContextMenuEvent)
keyPressEvent(QKeyEvent)        # 仅当 Item 拥有焦点
keyReleaseEvent(QKeyEvent)
focusInEvent(QFocusEvent)
focusOutEvent(QFocusEvent)
dragEnterEvent(QGraphicsSceneDragDropEvent)
dragMoveEvent(QGraphicsSceneDragDropEvent)
dropEvent(QGraphicsSceneDragDropEvent)
hoverEnterEvent / hoverMoveEvent / hoverLeaveEvent  # 需开 ItemAcceptsHoverEvents
inputMethodEvent(QInputMethodEvent)               # 输入法
```

------------------------------------------------
三、状态 & 变换
```python
itemChange(GraphicsItemChange, value)  # 万能“属性变化”钩子，常用来监听 position/selected/shape 变化
setSelected(bool)
setVisible(bool)
setEnabled(bool)
setPos(QPointF)
setRotation(qreal)
setScale(qreal)
setTransform(QTransform)
```

------------------------------------------------
四、碰撞 & 选择
```python
collidesWithItem(QGraphicsItem, mode)
collidesWithPath(QPainterPath, mode)
contains(QPointF)               # 默认用 shape()
```

------------------------------------------------
五、生命周期 & 序列化
```python
type()                          # 返回用户自定义类型枚举
clone()                         # 若你要 copy
```

------------------------------------------------
六、实践
> **“bounding + paint 必须写，**  
> **mouse/key/drag 按需拦，**  
> **itemChange 万能钩，**  
> **其余事件看标志。”**



### 补充常用缺口

1. **Scene / View** 共用
   `focusNextPrevChild(bool next)`   # Tab/Shift-Tab 焦点切换，常被忘记重载
2. **Scene 专属**
   `sceneRectChanged(QRectF)`        # 信号，不是虚函数，但文档里常列在一起
   `selectionChanged()`              # 同样是信号，用户经常监听
3. **Item 专属**
   `prepareGeometryChange()`         # 不是事件，但必须手动调，否则 boundingRect 改动不刷新
   `shape()`                         # 你已列出，但加一句“默认返回 boundingRect()，想精确碰撞才重写”



### 信号和虚函数  

- **虚函数** = “Qt 框架**直接调你**” → 用来**介入处理流程**；  
- **信号** = “ Qt 框架**广播消息**” → 用来**被动监听变化**；  
- 还有 **property/notify**、**eventFilter**、**QObject::event** 等“旁路”机制，但日常最多就是**虚函数 + 信号**两条主线。

------------------------------------------------
一、虚函数（virtual / override）

**谁发** | 框架内部**硬编码**调用  
**谁写** | 用户**重载**  
**目的** | **改变或扩展** Qt 的默认行为  
**例子** |
```cpp
void mousePressEvent(QMouseEvent *e) override;
void paint(QPainter *painter, …) override;
```
**特点** |
- 你不写，Qt 走默认实现  
- 写了就必须 `accept()` / `ignore()` 或 `super()` 决定链是否继续  
- **编译期绑定**（vtable）

------------------------------------------------
二、信号（signal）

**谁发** | 任何**QObject 派生类**里 `emit mySignal()`  
**谁写** | 用户**声明**即可，moc 自动生成实现  
**目的** | **通知外界**“我变了”，自己不干预流程  
**例子** |
```cpp
signals:
    void selectionChanged();
```
**特点** |
- **运行时连接** (`connect`)  
- 可 1-N、N-1、队列/直连  
- 不返回值，只广播

------------------------------------------------
三、其他“旁路”机制（了解即可）

1. **property + NOTIFY**  
   ```cpp
   Q_PROPERTY(int x READ x WRITE setX NOTIFY xChanged)
   ```
   - 不是虚函数，也不是信号，但**值变化时自动 emit NOTIFY 信号**  
   - QML/设计师层用得最多

2. **eventFilter**  
   - 不继承、不重载，**任意 QObject 都能监视另一个 QObject 的所有事件**  
   - 适合“横切”逻辑（日志、快捷键、全局水印）

3. **QObject::event()**  
   - 比虚函数更底层，**所有事件（含未公开类型）**都路过这里；  
   - 一般只用于“没对应虚函数”或“必须提前拦截”的场景

------------------------------------------------
> **“虚函数 = 改流程，信号 = 听广播；其余旁路看需求。”**

日常写图形程序：  
- **重载虚函数** → 处理绘制、交互、碰撞；  
- **连接信号** → 监听选择、位置、生命周期变化；  
- **eventFilter / property NOTIFY** → 跨模块、全局、QML 数据绑定。