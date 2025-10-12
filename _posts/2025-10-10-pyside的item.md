> scene/view体系是神器.

在 PySide 的 Graphics View 框架中，所有能被放进 `QGraphicsScene` 的对象都继承自 `QGraphicsItem`。

一、3 条继承线（Qt 官方分类）
1. 基本图形项（QGraphicsItem）  
   只提供几何形状，不能单独作为窗口控件。
2. 控件项（QGraphicsWidget）  
   继承自 QGraphicsItem，但同时具有 QWidget 的窗口语义（布局、调色板、字体、焦点策略等）。  
   可嵌入普通 QWidget（见第 3 类）。
3. 布局/窗口包装器  
   QGraphicsWidget 的配套类，用来把普通 QWidget 塞进场景，或在场景里做布局。

------------------------------------------------
二、具体类与用途速览

| 大类       | 类别             | 类名                                     | 典型用途 / 特点                             |
| ---------- | ---------------- | ---------------------------------------- | ------------------------------------------- |
| 纯item     | 基础形状         | QGraphicsRectItem                        | 矩形 / 圆角矩形                             |
|            |                  | QGraphicsEllipseItem                     | 椭圆 / 圆                                   |
|            |                  | QGraphicsLineItem                        | 直线段                                      |
|            |                  | QGraphicsPolygonItem                     | 任意多边形                                  |
|            |                  | QGraphicsPathItem                        | 任意 QPainterPath（贝塞尔、合并形状等）     |
|            | 文字             | QGraphicsSimpleTextItem                  | 单行纯文本，轻量                            |
|            |                  | QGraphicsTextItem                        | 富文本、可选、可编辑，支持 HTML             |
|            | 图像             | QGraphicsPixmapItem                      | 显示光栅图（QPixmap）                       |
|            |                  | QGraphicsSvgItem                         | 显示矢量 SVG（QtSvg 模块）                  |
| widget桥   | 提供基类         | QGraphicsWidget                          | 场景内的“窗口”，支持布局、样式表、焦点链    |
|            |                  | QGraphicsProxyWidget                     | 把任何 QWidget 包装成 item 放进场景         |
| widget布局 | 只能布局widget桥 | QGraphicsLinearLayout                    | 场景内水平/垂直布局                         |
|            |                  | QGraphicsGridLayout                      | 网格布局                                    |
| 基类       | 自定义           | 继承 QGraphicsItem（或 QGraphicsObject） | 重写 paint()/boundingRect()，想画什么画什么 |

------------------------------------------------
三、与场景/视图的关系
- QGraphicsScene 只是一个“无限大”的管理器，负责保存、索引、事件分发。  
- QGraphicsView 负责把场景的一块区域渲染到屏幕，可多个视图同时观察同一 scene。  
- 任何 item 都通过 scene.addItem() 或各自工厂方法（addRect、addText…）放进场景；视图会自动重绘。
  - 特别注意, 工厂方法生成的不是'正常'item, 做事件链接会失效.


------------------------------------------------
13太保: 形状 5 种（矩、椭、线、多边形、路径） + 文字 2 种 + 图像 2 种 + 控件 2 种 + 布局 2 种。  
再加上一个“自定义”接口(继承 QGraphicsItem)，足够覆盖从简单矢量图到复杂嵌套窗口的全部需求, 且框架开销非常低。

### 原理性要点: item不是qobject

- **没有 `signal/slot`**
- **没有 `deleteLater()`**
- **没有 `setProperty()/property()`**
- **不会被 `QTimer.singleShot(0, obj, slot)` 这类“槽”调用**
