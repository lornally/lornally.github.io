太奇怪了,QGraphicsView.event里面不调用super, 就会导致, scene0,0的位置不在中心, 各种事件处理不生效, 滚动条没有, 滚动当然也不响应, 为什么不调用super会有如此严重的后果?

kimi说他不知道, grok说: 

**原因：** 重写`event()`不调用`super()`绕过QGraphicsView的默认事件分发链（继承自QAbstractScrollArea），中断事件转发到具体处理器（如`wheelEvent`、`resizeEvent`、`showEvent`）。这导致：滚动条未更新范围/响应（无`scrollContentsBy`）；场景(0,0)未居中（无自动`centerOn`或变换锚点应用）；手势/滚动事件未处理（无转发到场景/项，破坏连续状态如`GestureUpdated`）。重写event()而不调用super()中断了Qt事件分发链，跳过QGraphicsView（继承自QAbstractScrollArea）的默认处理器，导致滚动、缩放、居中等功能失效。

也就是说: QGraphicsView的默认状态, 依赖于他自己的event虚函数的处理. 太怪异了.....