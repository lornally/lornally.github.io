> 一开始看到electron采用了semver spec, 也没多想就放过了. 但是, 当我自己写package.json的时候, 发现版本号那个地方也是这个货.

约定:

1. 公开发布的版本new product: 1.0.0 
2. 补丁版本patch release: 1.0.1 
3. 小版本minor release: 不打破目前的各种约定1.1.0 
4. 主版本major release: 打破向后兼容2.0.0 

如果你搞了一个补丁版本package 1.0.4,如何描述这个版本的范围呢?

- 兼容所有补丁版本Patch releases, 可以写成:  `1.0` 或者 `1.0.x` 或者 `~1.0.4` 这三种写法都是一个意思.
- 兼容所有小版本Minor releases可以写成:  `1` 或 `1.x` 或 `^1.0.4` 
- 兼容所有主版本Major releases可以写成:  `*` 或 `x` 或者 latest

> 结案陈词: 奶奶的, 太装逼了. 为啥要用符号呢? ~^, 脑子有毛病吧?