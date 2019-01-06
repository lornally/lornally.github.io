> ​	git@github.com:codemirror/CodeMirror.git

###### 先说初始版本: b630af89bc80aea627401595b51d5552ab8b8410

1. 光标位置: 

```js
mouseEventPos: function (e) {
      var off = eltOffset(this.lines[0].div),
        x = e.e.pageX - off.left + this.code.scrollLeft,
        y = e.e.pageY - off.top + this.code.scrollTop;
      return {
        line: Math.floor(y / this.lineHeight()),
        ch: Math.floor(x / this.charWidth())
      };
    },
```

marijn这里是硬算的, 他按照定宽字体算的. 这个不成, 东亚字符集就完蛋了.

2. 选择这块
   - 是通过跟踪event: mousedown和mouseup来做的.
   - mousedown的时候跟踪move up leave三个事件.
   - mouseup的时候和keypress一样, 使用onactivity
   - 行位置, 不停的比较, 然后再textarea里面放入足够多的行: 比选择内容上下各多一行.
   - 行内位置再mousedown处理,  实际是mouseeventpos, 硬算出来的. 所以选择这个事也就不准了.

看到这里, 估计后续版本作者不这么干了, 我也很好奇, 后续版本, 作者如何拿到合理的行数和字数.

###### 那么我们继续向后阅读源代码

- 42cad3c0版本, 中文依旧是问题.
- beta1版本, 1500行, 已经没有mouseEventPos了, 但是, 中文依旧是问题.

```js
function posFromMouse(e) {
    var off = eltOffset(lineWrap),
        x = e.pageX() - off.left,
        y = e.pageY() - off.top;
    if (e.target() == code && y < (lines.length * lineHeight())) return null;
    var line = showingFrom + Math.floor(y / lineHeight());
    return clipPos({line: line, ch: charFromX(clipLine(line), x)});
}


function charFromX(line, x) {
    var text = lines[line].text, cw = charWidth(); //这里还是硬算的.
    if (text.indexOf("\t") == -1) return Math.min(text.length, Math.round(x / cw));
    var mspan = measure.firstChild, mtext = mspan.firstChild;
    try {
        var from = 0, to = text.length;
        for (;;) {
            if (to - from <= 1) return from;
            var middle = Math.ceil((from + to) / 2);
            mtext.nodeValue = text.slice(0, middle);
            if (mspan.offsetWidth > x) to = middle;
            else from = middle;
        }
    } finally {mtext.nodeValue = "-";}
}
```

- beta2依旧无法正确处理中文. 位置函数没有大变化.

```js
function charFromX(line, x) {
    var text = lines[line].text, cw = charWidth();
    if (text.indexOf("\t") == -1) return Math.min(text.length, Math.round(x / cw));
    var mspan = measure.firstChild, mtext = mspan.firstChild, old = mtext.nodeValue;
    try {
        var from = 0, to = text.length;
        for (;;) {
            if (to - from <= 1) return from;
            var middle = Math.ceil((from + to) / 2);
            mtext.nodeValue = text.slice(0, middle);
            if (mspan.offsetWidth > x) to = middle;
            else from = middle;
        }
    } finally {mtext.nodeValue = old;}
}
```

- v2.0依旧没有解决中文问题.

```js
function charFromX(line, x) {
    var text = lines[line].text, cw = charWidth();
    if (x <= 0) return 0;
    if (text.indexOf("\t") == -1) return Math.min(text.length, Math.round(x / cw));
    var mspan = measure.firstChild, mtext = mspan.firstChild, old = mtext.nodeValue;
    try {
        mtext.nodeValue = text;
        var from = 0, fromX = 0, to = text.length, toX = mspan.offsetWidth;
        if (x > toX) return to;
        for (;;) {
            if (to - from <= 1) return (toX - x > x - fromX) ? from : to;
            var middle = Math.ceil((from + to) / 2);
            mtext.nodeValue = text.slice(0, middle);
            var curX = mspan.offsetWidth;
            if (curX > x) {to = middle; toX = curX;}
            else {from = middle; fromX = curX;}
        }
    } finally {mtext.nodeValue = old;}
}
```

- v2.01完美解决中文问题, 学习一下.

```js
function charFromX(line, x) {
    if (x <= 0) return 0;
    var lineObj = lines[line], text = lineObj.text;
    function getX(len) { //按照字符长度拿到实际宽度.
        measure.innerHTML = "<pre><span>" + lineObj.getHTML(null, null, false, len) + "</span></pre>"; //这里是按照字符数拿到lineobj这一行的字符.
        return measure.firstChild.firstChild.offsetWidth; //这里拿到的就是span的宽度.
    }
    var from = 0, fromX = 0, to = text.length, toX; //to是这一行的总长度.
    // Guess a suitable upper bound for our search.
    var estimated = Math.min(to, Math.ceil(x / stringWidth("x"))); //随便找个字符, 毛估估字符数, 这个和之前的做法是一致的, 不过之前很粗暴的直接就用这个了, 这里对这个数字要进行两轮修正.
    for (;;) {//第一轮. 按照这个毛估估出来的数字找宽度. 
        var estX = getX(estimated);
        if (estX <= x && estimated < to) estimated = Math.min(to, Math.ceil(estimated * 1.2)); 
        //如果估计的数字宽度比x(应该的宽度)小, 并且估计的字符数比这一行的长度小, 那么就增加, 这个地方其实不用这么麻烦, 直接加一就好了.
        else {toX = estX; to = estimated; break;}
        //否则 tox就是估计宽度. to修正为估计字符数. 这个是估计上限.
    }
    if (x > toX) return to; //如果实际宽度大于估计宽度, 那么可以把这个估计宽度返回去了. 这个代表实际点击位置超过了行尾. 除了这种情况只为, 按照刚才的迭代, 一定是估计宽度大于x实际宽度.
    // Try to guess a suitable lower bound as well.
    estimated = Math.floor(to * 0.8); estX = getX(estimated);
    //按照估计宽度的0.8再估计一个宽度. 定好from和fromx(这个是估计下限), 这地方有bug吧, 如果这个下限还是高, 咋办? 哦没事, 如果还是高, 就是初始值都是0. 这里真心是个优化, 不写也没关系.
    if (estX < x) {from = estimated; fromX = estX;}
    // Do a binary search between these bounds.
    for (;;) { //这里是最最核心的代码了.
        if (to - from <= 1) return (toX - x > x - fromX) ? from : to;
        //如果to和from之间小于一个字符, 那么就可以返回字符数了. 光标一定再这个字符的左边或者右边.        
        var middle = Math.ceil((from + to) / 2), middleX = getX(middle);
        //这里就是速降找宽度, 逐个字符的找到合适的宽度. 
        if (middleX > x) {to = middle; toX = middleX;}
        else {from = middle; fromX = middleX;}
    }
}
```

- 大概理解了, marijn一个字符一个字符的计算出来的. 一个字一个字的拿宽度. 懂了. 
- 这代码风格真心够唠叨的. 作者和我一样, 再不需要优化的地方写了一堆代码, 其实就是一个字符一个字符的实验宽度, 宽度够了. 就对了. 就找到位置了. 这种方式真心辛苦了, 作者可以说是硬干出来的.
- 然后咱们往后跳一下, 看看codemirror2最后一个版本的鼠标位置是怎么搞的.
- v2.33是主线上面最后一个2的版本, 虽然之后2还有几个版本, 但是都已经离开了主线.
- 2.33版本并无不同.

至此codemirror2已经阅读完了. 这个地方硬拿尺寸的办法, 学到了, 感谢marijn.