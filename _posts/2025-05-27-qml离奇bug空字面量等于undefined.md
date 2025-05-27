> 这bug可是太牛逼了, 很难相信他真的存在.

```json
pragma ComponentBehavior: Bound
// ds平铺.qml
import QtQuick
import QtQuick.Shapes

Item {
 
    property var nodePositions: {} 
    Component.onCompleted: {
        console.log("根组件初始化完成，nodePositions:", nodePositions)
        // 此时输出undefined, 你敢信?
    }
}
```

破案了, 这语言特性太他妈搓了.

```json
property var nodePositions: {'a'}  // QML 解析为: { return 'a'; }
//qml默认{}不是字典字面量, 而是函数体, 等价于(), 逆天啊....
property var nodePositions: ({}) //这里是防御性编程

```





##### 第二个离奇bug

* 自定义组件使用mapToItem计算坐标错误, 要么他不生效, 要么他给的都是统一的值. 
* 官网有一个解决方案令人发指, 换成: mapFromItem, 但是这个其实没有卵用.
* 所有ai此时都会乱分析, 他们都认为是加载顺序问题...这再次增加了问题解决的难度.
* 实际问题很简单:

```json
// 简单的自定义组件
Component {
Column {
  anchors.centerIn: parent // * 神啊这一行是关键, 如果没有准确定位, 那么位置就是会计算出一个莫名的位置, 然后, 不会有任何地方报错.
  ...
}}

//加载组件的代码
Loader {
  sourceComponent: testComponent
  anchors.centerIn: parent
  onLoaded: {
      item.anchors.centerIn = item.parent // 显式同步坐标系
      // 这里的 item 是 testComponent 的实例
  }
}
```

* 没给容器一个位置, 容器就瞎计算, 这语言特性你敢信?  真是活久见.... 如果这个位置那么重要, 你编译器给我报出来啊....

> 一天2坑, 还能活着都得感谢qt不杀之恩., 感觉自己是一个还不会走路的孩子, 家长给买了一个独轮车, 还把咱扶上了车....

* 草率了, 实际情况相当复杂. 最终还是ds解决问题, yyds啊.....

###### 展示真正的技术

* 关键问题是组件的对齐问题, 之前的样例代码, loader居中了, 因此column也要居中. 这样才能对齐. 如果采用下面代码, 就不需要居中对齐了:

```json
// 这样是左上角对齐.
Component.onCompleted: {
  testComponent.createObject(root)
}

// 这样也是左上角对齐.
Repeater {
  model: 1
  delegate: testComponent
}

```

* 所以问题的关键就是找到关键的问题, 外部初始化组件的对齐方式和组件自身的对齐方式必须一致, 如果不一致, 那么就要进行换算, 或者在中间包一层.





