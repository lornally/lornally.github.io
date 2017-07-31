### 声明

```swift
var xxx : Int? = 200
xxx = nil
var ooo: String?
```

### 强制解析

```swift
let ppp = xxx! // 这里保证xxx有值
if xxx != nil //如果不能保证xxx有值 那么可以判断xxx是否有值
```

### 可选绑定

```swift
if let qqq = xxx {
  // 这里可以任意使用qqq, 一定是qqq有值, 才会执行这个代码块.
}else{
  // 这里就是xxx为nil的时候会执行的代码了.
}
```

### 隐式解析

```swift
let possibleString: String? = "An optional string." //可选类型
let forcedString: String = possibleString! // 需要感叹号来强制解析获取值

let assumedString: String! = "An implicitly unwrapped optional string."// 隐式解析
let implicitString: String = assumedString  // 不需要感叹号
```

### 运行时错误

1. 如果你在隐式解析可选类型没有值的时候尝试取值，会触发运行时错误。
2. 如果你在没有值的普通可选类型后面加一个惊叹号来强制解析, 也会出发运行时错误.

### 断言

```swift
assert(age>=0,"这里是断言的原因啥的, 方便debug") //如果断言成功, 程序可以继续进行, 断言失败, 程序终止, 这个是debug利器, 比php中的dd好用.
```

