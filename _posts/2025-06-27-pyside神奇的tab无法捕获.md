>   * tab键第一次正常建立节点, 并进入编辑模式, 然后报一个输入法错误, 然后不会再响应任何tab了, 无法捕获他.

* bug详细描述
  * enter也会引起imk错误, 然后, tab也不能用了
  * 空格也会引起imk错误, 然后, tab也不能用了
  * 错误:  error messaging the mach port for IMKCFRunLoopWakeUpReliable
  * 换成默认输入法, 报错换成了:  Python[83634:32490113] TSM AdjustCapsLockLEDForKeyTransitionHandling - _ISSetPhysicalKeyboardCapsLockLED Inhibit
* 总结: 任何键盘输入都会导致一个报错, 此报错之后, 所有的tab按键就没有任何响应了, keyPressEvent不会被触发
* 如果在编辑模式打开输入法, 退出编辑模式就关闭输入法, 是否是更简洁明了的方案?



###### 解决方案一:

全局事件:

```py
    class GlobalEventFilter(QObject):
      def eventFilter(self, obj, event):
          if event.type() == QEvent.Type.KeyPress:
              print(f"全局事件捕获: {event.key()} 对象: {obj}")
              return my_keypress_handler(obj, event) 

          return super().eventFilter(obj, event)

    global_filter = GlobalEventFilter()
    app.installEventFilter(global_filter)
```

###### 解决方案二:

搞focus

```py
    dummy = QGraphicsRectItem()
    dummy.setFlag(QGraphicsItem.GraphicsItemFlag.ItemIsFocusable, True)
    item.scene().addItem(dummy)
    dummy.setFocus()
    item.scene().removeItem(dummy)
```





### 尝试了, 没用

```

    # QApplication.inputMethod().hide()    
    # QTimer.singleShot(0, node.setFocus)
```

```
   # QApplication.processEvents()
   # print(f"当前输入法状态: {QApplication.inputMethod().isVisible()}")
```

```
    # self.setFocusPolicy(Qt.FocusPolicy.StrongFocus)

```

```
   # view.setTabKeyNavigation(False)  # 禁用Tab焦点切换
   # ✅ 对整个窗口禁用Tab导航
   # window.setTabOrder(None, None)  # 清除Tab顺序
    # view.setAttribute(Qt.WA_InputMethodEnabled, False)
    view.setAttribute(Qt.WidgetAttribute.WA_InputMethodEnabled, False)


```

