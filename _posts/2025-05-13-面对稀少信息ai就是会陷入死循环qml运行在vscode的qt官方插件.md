> * 稀少的正确信息, ai无法识别

怎么组织提示词都没用: 

```md
问题描述: macos15上面安装了qt6, 然后在vscode(1.100)上搞了python的虚拟环境. 安装了pyside6, 跑qml.   插件使用Qt group的qt extension pack(1.2), id: theqtcompany.qt, 此时打开任意qml, lint报错:  Failed to import QtQuick, 此种问题一般会出现在插件供应商的论坛或者git issue中, stackoverflow应该也有解决方案, 请帮忙搜索下.
```

此时, 所有ai都是一通蛮干. grok, gemini, claude, deepseek....

其中答案靠边的: claude, 但是, 这种配置问题, 失之毫厘谬以千里, 继续追问

```
你这是一通改, 咱能不能先确认下相关参数是否真的有问题, 是否真的需要这么改? 我的macos, qt, vscode, pyside, qt group的vscode插件, 都是最新版, 都是2025年3月之后更新的. 所以版本不是问题, 应该就是某个设置.  lint还提示CMake variable, 是不是要配置cmake给vscode的qt插件用?
```

ai继续胡说八道, 最终是怎么解决的呢? 请看vcr:

1. 参考官网: https://www.qtcentre.org/threads/72218-Using-VSCode-with-QML

   * 答案明晃晃, 让ai搜索, 他却搜索不到.

2. 唯一的手动操作:

   ```sh
   # .vscode中的两个文件是插件自动建立的: 
   #  把那个qmxxx.ini 加上   importPaths=/Users/bergman/Qt/6.9.0/macos/qml
   ```

3. 重启vscode

   1. 右下角会提示配置qt目录, 或者安装qt, 你选择配置, 他自己就配好了. 点一下的事.

4. 点击任何一个qml文件

   1. vscode头部的命令面板弹出, 配置一个编译器, 此时选择qt安装目录的那个, 一般是最后一个. 

5. 这个世界清净了. 

> 当我们面对自动化的时候, 一切正常确实不费心, 但凡有一点不正常, 那都比没有自动化更糟糕. 解决这种问题的能力, 应该是学校教学的主要目标, 而不是来到社会上才开始学. 课本上的正确答案啥时候都能学, 异常问题, 超限问题, 解决这些问题, 才是一个人的核心能力. 什么是人? 奔跑, 投掷, 解决问题.....



###### 对于那个手动操作需要一点补充

* 配置的目录: importPaths=/Users/bergman/Qt/6.9.0/macos/qml
* 这个目录是qml module的目录, 不是qml shell的目录. 是库不是脚本.
* claude的问题就在这里, 他把脚本配到这个环境变量, 当然不正确了.
* 然后, 关键是, 他死活无法认识到这一点, 然后就进入垃圾循环时间.
* 这里面比较可惜的是ds, 在类似问题上ds的表现一贯是最好的. 但是, 这个问题的解刚刚出现了1个多月, 所以ds无法感知到, 这是ds本身的问题. 他的感知和记忆能力比其他模型差, 但是, 他的推理能力确实强, 而我们都知道, 感知和记忆只要堆参数, 推理能力不是的. 

###### 再次补充, 如果不是项目目录怎么办

* 夫人, 你也不想打开一个qml就看到满屏lint错误吧?
* 没问题, 两个设置. 我是都选了, 或许一个也行....

```ini
# vscode设置
# 搜索importpath
1. addition import paths 此处贴入你的路径: importPaths=/Users/bergman/Qt/6.9.0/macos/qml
2. use qml_import_path 环境变量, 这个勾在我这没啥效果. 不挑也无所谓.
```

