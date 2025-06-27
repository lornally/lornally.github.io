> 想把代码展示在某个屏幕上, 但是, 遇到了'假'函数

这个代码不会生效

```py
screen = app.screens()[1]
window.setScreen(screen) 
```

ai会很傻叉的建议你调整各种代码间的顺序, 然并卵, 例如:

* window.setCentralWidget
* window.showMaximized/show
* scene.setSceneRect
* view.centerOn

这个代码有效: 

```py
screen = app.screens()[1]
window.move(screen.geometry().topLeft())
```



结论: 

* window.setScreen(screen) 是一个假的函数.
* 他在windows或许可以用.
* 他在linux上取决于你的窗口环境.
* 他在mac上基本没用.
* 这就是为什么他是window的函数, 他只兼容windows中的一部分window. 呵呵呵呵呵.......
