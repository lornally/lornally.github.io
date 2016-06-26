> 挺好玩的, 从来没有想过, markdown的流程图和表格其实也很简单的.

### 表格

```markdown

| typora竟然 | 支持             | 表格.     |
| -------- | -------------- | ------- |
| 那么       | 很多需求           | 都可以解决了. |
| 至少象限图    | 以后也用markdown了. |         |
|          |                |         |
```

> 如果使用表格, 建议用typora, 直接菜单选择表格, 超级简单.

| typora竟然 | 支持             | 表格.     |
| -------- | -------------- | ------- |
| 那么       | 很多需求           | 都可以解决了. |
| 至少象限图    | 以后也用markdown了. |         |
|          |                |         |

流程图

talk is cheap, show the code: 

```markdown
​```flow   //这种语法等于code形式
st=>start: Start
e=>end
op=>operation: My Operation
cond=>condition: Yes or No?

st->op->cond
cond(yes)->e
cond(no)->op
​```
```

```flow
st=>start: Start
e=>end
op=>operation: My Operation
cond=>condition: Yes or No?

st->op->cond
cond(yes)->e
cond(no)->op
```
#### 解释:

##### 六个类型:

1. start
2. end 
3. operation 
4. subroutine 
5. condition 
6. inputoutput

##### 特殊标志

1. 开始
   - st=>start: 开始
2. 条件
   - cond=>condition: 确认？
3. 结束
   - e=>end: 结束

##### 流程

这个超级简单: 

st->op->cond    

> 这样就定义了从st到op到cond的一个3步的流程.





> 后记, 好吧, 貌似jekyll默认不支持flow, 需要插件, 不折腾了.
>
> 再叹一声, jekyll也不支持table, 好吧.

