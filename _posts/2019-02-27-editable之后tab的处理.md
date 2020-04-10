###### 测试tab

- tab可以插入和显示, 但是好短.  问题来了, 如何让tab看起来有4个空格那么大(浏览器里面2个空格都很小的)?
- 原因找到了, 不论是多少个空格或者tab, 在html里面都会处理为一个单独的空格, 这个是原本的规范.

1. 需要先解决vscode的问题, 
   1. vscode会自动转化tab为空格. 关键字: tab.
   2. 并且vscode再保存文件的时候的格式化, 还会无差别处理div里面的tab, 也不利于我的测试. 也需要关掉, 关键字: save.
2. 然后, pre里面的tab确实生效了, 而且默认是4个空格. 但是editable的div里面的tab依旧没有生效.

tab的编码:

```js
let tab='\t';
tab='&#9;';
// ASCII character 9, or Unicode U+0009.
```



绕行(这是一个可行的方案):

1. nbsp
2. ensp 2空格
3. emsp 4空格

>  参考: https://www.quora.com/How-do-you-add-a-tab-space-in-HTML

由此产生了一个疑问?  如果用pre +contenteditable, 可以吗?  答案竟然是可行. 那么针对tab我们有两个答案了: 

1. 使用pre +contenteditable, 这个tab表现完美, 其他表现不知道, 随缘吧.
2. 使用emsp, 貌似解决问题, 反复转码之后, 不知道是个神马玩意了. 
3. 竟然发现了方案三: execCommand('indent', false, null) 尝试一下. 这个问题很明显, tab只是加在行首, 中间加不进去.  而且实际上添加了blockquote元素

> tab暂时告一段落.   才叫见鬼…..

4. 因为我有发现了各种解决方案比如: white-space: pre,  white-space: pre-wrap, 这个是完美的解决方案, 完美的令人发指……, 这是已知的终极解决方案.

###### tab的最终解决方案

```css
.tabstop {
    -moz-tab-size: 40;
    -o-tab-size: 40;
    tab-size: 40;
    white-space: pre-wrap;
}

.whitespace {
    white-space: pre;
    -moz-tab-size: 40;
    -o-tab-size: 40;
    tab-size: 40;
}
tab编码: '\t';'&#9;';// ASCII character 9, or Unicode U+0009.
```

white-space: pre和pre-wrap的区别:

- pre will only wrap on line breaks so pre-wrap is probably preferable to avoid a horizontal scroll bar.
- 因此我们用pre-wrap是最合理的.

> 除了万能的Stack Overflow, https://codeitdown.com/css/tab-size/
>
> https://stackoverflow.com/questions/2237497/how-to-make-the-tab-key-insert-a-tab-character-in-a-contenteditable-div/2237548#2237548    这里面的meouw的答案.

