手贱升级了sierra, 忽然发现karabiner失灵了, 其实, 这种事我们遇到过很多, 结局一般都是抛弃掉那个软件. 

但是, google搜索一下, 我惊呆了.

官方已经转为sierra开发了karabiner element. — 是的改名了, 你没有看错.

赶快装上, 需要配置json, 没关系官方提供了很多样例, 其实都一样, 看一个就可以了, 然后, 再配合官方提供的按键表, 万事大吉.

配置sh代码:

```sh
mkdir -p ~/.karabiner.d/configuration/
cd ~/.karabiner.d/configuration/
touch karabiner.json
open karabiner.json -a atom #用atom打开, 我的默认编辑器不是atom
```

配置样例:

```json
{
    "profiles": [
        {
            "name": "Default profile",
            "selected": true,
            "simple_modifications": {
                "left_command": "left_option",
                "right_command": "right_option",

                "left_option": "left_command",
                "right_option": "right_command"
            }
        }
    ]
}#json 的括号配对, 永远都是一个问题啊.
```



### 参考

主页更新: https://pqrs.org/osx/karabiner/

新软件的下载和主页: https://github.com/tekezo/Karabiner-Elements

如何修改到json: https://github.com/tekezo/Karabiner-Elements/tree/master/usage

json修改的案例: https://github.com/tekezo/Karabiner-Elements/tree/master/examples

blog写了按键表:  https://lisplog.org/karabinerelements_for_macos_sierra.html

https://github.com/tekezo/Karabiner-Elements/blob/master/src/share/types.hpp#L177-L369

原始的karabinar可以控制修改哪个device: https://pqrs.org/osx/karabiner/xml.html.en#devicedef

 原始的karabinar可以: disable key-mapping for the internal keyboard for Apple keyboards: don't remap apple's keyboards

原始karabinar的manual: https://pqrs.org/osx/karabiner/document.html.en#profiles