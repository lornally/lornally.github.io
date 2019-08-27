- https://medium.com/content-uneditable/lessons-learned-from-creating-a-rich-text-editor-with-real-time-collaboration-c35870046987
  - 这里就是描述了那个干净的编辑器:) 它是一个协同编辑器. 他对协同做了相当深入的讨论.
- https://florian.rivoal.net/blog/2016/12/content-editable-is-a-scary-place/
- 另外, 推荐阅读codemirror/prosemirror的文档, 作者描述了他的苦痛.
  - code1是editable, 但是, 被坑死了.  有2篇文档.  其实主要是浏览器兼容, 尤其是IE对标准的不支持.
  - code2-5抛弃了editable, 但是, 他有迈不过去的问题. 这里同样偶2篇文档
  - prose改回了editable, 作者发现问题似乎不存在了(感谢微软挥刀自宫). 这里依旧是2篇文档.
  - code6也决定回归editable. 这里没有文档. 但是我们可以期待.
  - 顺便说一句, 我的blog里面有详细的阅读笔记. 

###### cke的角度

两种传统技术

- Operational transformation (OT) 
- conflict-free replicated data type (CRDT)

###### Operational transformation (OT) 

- 是apache wave和google docs的背后的技术.
- 这是一个传统的更新技术, wiki的一幅图解释的很清楚: https://en.wikipedia.org/wiki/File:Basicot.png
- 简单的说要根据本地操作, 修改远程操作的含义为'本意'.
- 有cc和cci模型
  - **C**ausality preservation因果本意保护, 保证执行顺序和原始的顺序一直, 从而保证结果的正确性.
  - **C**onvergence交点趋同一致, 如果所有人都没有修改文档, 则此时所有文档是一致的.
  - **I**ntention preservation意图缓存, 保证在任何文档状态下(被操作物存在), 一个操作的结果是一致的. 这里处理的是不能序列化的操作. 
- '本意'或者说'意图保留'有三个层次
  - 协作编辑的一致性(本意).
  - 内容操作的前置和后置操作, 符合通用的操作的本意.
  - 纯文本的两个基础操作的本意: 删除和插入.
- csm模型
  - 因果本意
  - Single-operation单操作效果本意
  - Multi-operation多操作效果本意
- ca模型
  - c因果本意
  - **A**dmissibility 可允许的操作, 就是不破坏因果性本意的操作.
- ca模型的收敛性最强, 导致算法的空间和时间开销都降低了, 不必维护总表. 这其实是rebase的思路.
- 最终结论是OT实际上基本无法实施. 目前是被放弃的状态.
- CRDT是OT的一个替代方案.

###### conflict-free replicated data type (CRDT)

- 目标是: 乐观复制, 简单的说, 就是接受所有修改意见.
- 举个例子: 只有一位数据, 真代表发生过, 假代表从未发生, 所有的数据集合起来, 只要有一个真, 那么结果就是真. 这里或运算起到了乐观复制的作用.
- CRDT的目标是给乐观复制一个可用的数据结构. 如果一个数据结构可以进行乐观复制, 那么我们就认为这种数据结构是crdt的一种.
- crdt是对cap的补充, 某种程度上满足cap的不严格的要求. 关于cap参见:  https://en.wikipedia.org/wiki/CAP_theorem
- crdt是数学家的胜利, 纯粹的从群论/集合论的角度提出的不冲突操作的要求, 满足交换, 结合, 等幂的集合, 数学上叫'半格', 这样的数据结构就可以做乐观复制.

crdt有两个类型:

- 操作型 这个似乎是帧同步?
- 状态型

他们是等价的, 但是各有优缺点, 

操作型crdt  cmrdt

- 这些操作必须是可交换的. 所以可以以任何顺序传递.
- 这些操作不是幂等的, 因此不可以有重复.

状态型crdt   cvrdt

- 这个思路是发出完整的本地状态给其他副本.
- 状态通过函数来合并merge, 必须是可交换, 可结合并且幂等.

crdt的已知的数据结构

- G-counter 单增计数器 维护为一个总量已知的单增的队列, 合并的时候, 谁的id大用谁的. 比如点赞计数器, 点赞了就是+1.  有n个终端, 那么就是一个n长的数组, 每个终端都维护自己的计数, 合并的时候, 每个元素都取最大值, 查询时, 对整个数组做一次加和.

- PN-counter 正负计数器 维护两个计数器, 一个单增, 一个单减, 最终计数=单增-单减. 比如点赞计数器, 单独维护取消队列, 或者Stack Overflow这种, 可以点赞, 并且可以踩一脚的评价系统.

- G-set 单增set 一个集合记录所有元素的添加, 不能删除.

- 2p-set 两相set  两个set, 分别记录删除和添加, 都是单增的, 一旦某个元素放到了删除集合, 就永远也不能检索到了, 这里是删除优先于添加.

- LWW(last-write-win) - element - set 维护两个set, 一个添加一个删除, 集合中每个元素都有时间戳.  通过在删除集中添加元素, 可以对元素做删除, 判断一个元素是否最终存在有两个可能性: 

  1. 他在添加集, 不在删除集
  2. 他在添加集, 并且他在删除集的时间戳早于添加集的时间戳.

  时间戳相等时, 采用某个策略, 这个策略可以偏向于删除也可以偏向于添加. 和2p-set不同, 这个支持元素的的删除后再添加.

- or(observed-remove)-set用id取代时间戳的lww.

  - 学术界有多篇论文都是在探讨对此种算法的优化。但OR-Set在实践中最严重的问题是一旦同步通道出现延迟或者中断，很可能出现用户认为早已删除掉的字段在同步恢复之后再次出现。从工程实践角度讲，更优化的方案是使用时间戳作为unique tag，好处是易保证唯一性，同时自带单调递增属性，重复删除添加时不会生成大量tag
  - 参考: 很不错的中文资料: https://juejin.im/entry/5b922d336fb9a05ce7513cff

- 序列(sequence)crdt, 使用sequence, list, 或者ordered set的crdt.

##### CAP

cap的意思是有三个元素不能同时被满足:

- 一致性（**C**onsistency） （等同于所有节点访问同一份最新的数据副本）
- [可用性](https://zh.wikipedia.org/wiki/可用性)（**A**vailability）（每次请求都能获取到非错的响应——但是不保证获取的数据为最新数据）
- [分区容错性](https://zh.wikipedia.org/w/index.php?title=网络分区&action=edit&redlink=1)（**P**artition tolerance）（以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在C和A之间做出选择。）

crdt对CAP形成重大影响: https://www.infoq.cn/article/cap-twelve-years-later-how-the-rules-have-changed/



###### prosemirror的角度

- 参考http://marijnhaverbeke.nl/blog/collaborative-editing.html
- 作者使用了一点OT, 但是也发现了OT的问题, 因此, 大部分是作者自己的解决方案. 难为marijn了.
- 作者基于事实得出一个结论, 很多时候与其OT不如用git diff更合理. 因为, 自动merge出来的很可能是垃圾.
- 关于undo, 作者认为不该维护一个公用的history, 应该每个人维护自己的. 

###### 