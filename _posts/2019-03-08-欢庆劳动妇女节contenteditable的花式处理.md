注意: 我们欢庆的是劳动妇女节, 是精神和人格独立的女子, 经济基础决定上层建筑, 这个女子的经济也应该是独立的.

### 第一部分 inline-block

为了让editable不再捣乱, 人类发现了各种解药…..

```css
[ contenteditable="true"] {			/* 使用属性编辑器	 */
    border: 1px solid rgb(155, 72, 72);
    background: rgba(201, 167, 123, 0.555);
    height: 80%;
    display: inline-block;	/* 这个会导致不自动生成div啥的. 但是, 会有br*/
    width: 80%;   /* 这一条需要加进来, 因为display: inline-block;会导致整个块缩起来 */
    margin: auto;
    padding: 10px;
}
```

据说会有不好的影响, 但是我暂时没发现: https://stackoverflow.com/questions/18552336/prevent-contenteditable-adding-div-on-enter-chrome

这个答案很有意思, 还有光标操作的样例.

### 第二部分 pre 其实是一个考虑, 估计不仅仅tab正常. 

```css
[ contenteditable="true"] {
    /* 使用属性编辑器	 */
    border: 1px solid rgb(155, 72, 72);
    background: rgba(201, 167, 123, 0.555);
    height: 80%;
    display: inline-block;
    /* 这个会导致不自动生成div啥的. 但是, 会有br*/
    width: 80%;
    /*  这一条需要加进来, 因为display: inline-block;会导致整个块缩起来 */
    margin: auto;
    padding: 10px;
    -moz-tab-size: 40;
    -o-tab-size: 40;
    tab-size: 40;
    white-space: pre-wrap; /*保护性代码*/
}
```

```html
<pre contenteditable="true"></pre> 
<!-- 用了pre导致br都没有了, 这个或许真的是一条路 -->
```

