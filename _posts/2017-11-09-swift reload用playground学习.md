### 前言

1. wikipedia的介绍很棒:https://en.wikipedia.org/wiki/Swift_(programming_language)
2. swift的历史值得一看: https://www.wired.com/2014/07/apple-swift/

### 优势

1. 简洁, 编译器的强大导致了各种简洁, 没有类似头文件, 各种import, 各种默认方法等等, 关键是没有自动生成代码这件事.
2. playground, 编辑当时可以看到结果, 虽然light table(python clojure js)也可以, 但是, 编译速度和运行速度都很快, 虽然js也满足这个条件.




### 学习感悟

```swift
//else if 一般情况下不如if好用, 例如: 
for i in 1 ... 12 {
    if isBlocked || isBlockedLeft {
        turnRight()
    }
    moveForward()
    if isOnGem {
        collectGem()
    }
}//这个写法比else if 写法简洁, 而且更好理解
```

