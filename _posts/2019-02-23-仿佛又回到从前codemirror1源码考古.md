> codemirror1的源码, 每次都是简单的浏览, 从来没有认真的调试.

这次来点真格的, 我们一起读下codemirror1的源码.

###### 从2007年读起.

1. 一开始的codemirror还是相当简单的, 这大哥一开始也是喜欢写注释的, 后来就不知道注释都到哪里去了. 奇怪了, 我再看怎么都看不到任何注视了, 是我幻视了?
2. 需要注意的是, 一定要加上MochiKit才能正常运行.
3. 并且如果在chrome运行, 需要再文件目录起一个server才行.
4. 一开始没有监听任何事件, 初始版本就2个按钮.

###### init版本, github最原始版本mar 2007 dda02b6

- index.html只是包含了editframe.html
- editframe.html是空的
- Util.js是工具类
- highlight.js

```js
//highlight.js的主入口函数
function addHighlighting(id){
  var textarea = $(id);
  var iframe = createDOM("IFRAME", {src: "editframe.html", "class": "subtle-iframe", id: id, name: id});
  textarea.parentNode.replaceChild(iframe, textarea); //这里用ifram替换了textarea
  connect(iframe, "onload", stage2);//注册了载入函数
  iframe.style.width = "500px";
  iframe.style.height = "400px";

  function stage2(){ //这个就是载入函数.
    var fdoc = frames[id].document;
    fdoc.designMode = "on"; //开启了designmode.
    importCode(textarea.value, fdoc.body); //这个就是innerhtml的一次替换. 主要是处理回车和空格. 变成br和nbsp.
    highlight(fdoc.body);//主处理函数
  }
}
```

```js
function highlight(node){
  if (!node.firstChild)
    return;
  var dom = traverseDOM(node.firstChild);
  var parsed = parse(iconcat(dom));
  var split = splitBy(function(t){return t.type == "newline";}, parsed);
  var line = null;
  
  function partLength(part){
    return part.firstChild.textContent.length;
  }
  function correctPart(token, part){
    return part.firstChild.textContent == token.value && hasElementClass(part, token.style);
  }
  function shortenPart(part, minus){
    part.firstChild.textContent = part.firstChild.textContent.substring(minus);
  }
  function removePart(part){
    var nextpart = part.nextSibling;
    line.removeChild(part);
    return nextpart;
  }
  function tokenPart(token){
    return SPAN({"class": "part " + token.style}, token.value);
  }

  forEach(split, function(tokens){
    line = line ? line.nextSibling : node.firstChild;
    console.log("Line = " + scrapeText(line) + ", tokens = " + map(itemgetter("value"), tokens));
    var part = null;
    forEach(tokens, function(token){
      console.log("Token '" + token.value + "'");
      if (!part) part = line.firstChild;

      var tokensize = token.value.length;
      if (correctPart(token, part)){
        part = part.nextSibling;
      }
      else {
        line.insertBefore(tokenPart(token), part);
        while (tokensize > 0) {
          var partsize = partLength(part);
          if (partsize > tokensize){
            shortenPart(part, tokensize);
            tokensize = 0;
          }
          else {
            tokensize -= partsize;
            part = removePart(part);
          }
        }
      }
    });
  });
}
```

> 第一个版本的代码大概就看到这里, 我们可以看到code1的初始目标还是比较简单的: 做一个highlight. 并且, 也没有考虑格式分离这类事. 并且我们能看到作者大量使用正则. 

###### 后续几个小版本

合在一起说, 等出现大变化, 我们再单独分析.

- 去掉div, 用span和br处理整个文档.
- 改小bug.
- 和IE搏斗. 例如
  - 为了让ie正常, 不在引入iframe的html, 而是直接硬编码, 写到代码里面. 用了element.write("<div>这里是编码</div>") 
  - ie的空白字符, 比如回车是\r\n

我抽取了一个版本mar2007, bff2e25, 感觉designmode确实更好用. 我要实验一下.

> 重要结论: designmode的tab处理本身就是对的. contenteditable的tab就必须要处理了. 

那么我的疑问出现了, code1的最后一个版本是怎样的呢? designmode还是contenteditable?

###### 尝试阅读code1的最后一个版本.

js处理的文件是: jstest.html, 很神奇, 这个版本是可以直接运行的. 不需要serve, 也不需要mochikit.

```html
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <script src="js/codemirror.js" type="text/javascript"></script>
    <script src="js/mirrorframe.js" type="text/javascript"></script>
    <title>CodeMirror: JavaScript demonstration</title>
    <link rel="stylesheet" type="text/css" href="css/docs.css"/>
  </head>
  <body style="padding: 20px;">
<div>
<textarea id="code" cols="120" rows="30">
// Here you see some JavaScript code. Mess around with it to get
// acquainted with CodeMirror's features.

// Press enter inside the object and your new line will be suitably
// indented.
var keyBindings = {
  enter: "newline-and-indent",
  tab: "reindent-selection",
  ctrl_z: "undo",
  ctrl_y: "redo",
  ctrl_bracket: "highlight-brackets",
  ctrl_shift_bracket: "jump-to-matching-bracket"
};

// Press tab on the next line and the wrong indentation will be fixed.
      var regex = /foo|bar/i;

function example(x) {
  // Local variables get a different colour than global ones.
  var y = 44.4;
  return x + y - z;
}
</textarea>
</div>

<script type="text/javascript">
  var textarea = document.getElementById('code'); //拿到textarea.
  var editor = new MirrorFrame(CodeMirror.replace(textarea), {
      //看上去这里制造了一个frame
    height: "350px",
    content: textarea.value,
    parserfile: ["tokenizejavascript.js", "parsejavascript.js"],
    stylesheet: "css/jscolors.css",
    path: "js/",
    autoMatchParens: true
  });
</script>

  </body>
</html>

```

一共两个js, 我们先看看mirrorframe.js

```js
//入口主函数看上去就是这个.
function MirrorFrame(place, options) { //第二个参数只是给codemirror使用.
  this.home = document.createElement("div");
  if (place.appendChild) //place是第一个参数, 是codemirror给过来的. 现在去看看codemirror.
    place.appendChild(this.home);
  else
    place(this.home);

  var self = this;
  function makeButton(name, action) {
    var button = document.createElement("input");
    button.type = "button";
    button.value = name;
    self.home.appendChild(button);
    button.onclick = function(){self[action].call(self);};
  }

  makeButton("Search", "search");
  makeButton("Replace", "replace");
  makeButton("Current line", "line");
  makeButton("Jump to line", "jump");
  makeButton("Insert constructor", "macro");
  makeButton("Indent all", "reindent");

  this.mirror = new CodeMirror(this.home, options);//这里可以看到参数都给codemirror使用了. this.home就是这里第一行制造的div.
}
```

再看看codemirror.js, 第一步先看看上面直接用到的入口: CodeMirror.replace

```js
 CodeMirror.replace = function(element) {
    if (typeof element == "string")
      element = document.getElementById(element);
    return function(newElement) {
      element.parentNode.replaceChild(newElement, element);
    };
  }; //这里显然没写太多东西, 拿的东西再哪里? 恐怕就在于直接执行的代码. 但是还是要承认作者使用了高阶函数, 这个啥意思? 要查一下. 幸亏我有作者的书.
```

因为前面的没看明白, 所以看看codemirror是啥吧.

```js
//codemirror整体是一个闭包, 返回一个codemirror. 从121-178行定义了这个对象的主体.
function CodeMirror(place, options) {
    // Use passed options, if any, to override defaults.
    this.options = options = options || {};
    setDefaults(options, CodeMirrorConfig);

    // Backward compatibility for deprecated options.
    if (options.dumbTabs) options.tabMode = "spaces";
    else if (options.normalTab) options.tabMode = "default";
    if (options.cursorActivity) options.onCursorActivity = options.cursorActivity;

    var frame = this.frame = createHTMLElement("iframe");
    if (options.iframeClass) frame.className = options.iframeClass;
    frame.frameBorder = 0;
    frame.style.border = "0";
    frame.style.width = '100%';
    frame.style.height = '100%';
    // display: block occasionally suppresses some Firefox bugs, so we
    // always add it, redundant as it sounds.
    frame.style.display = "block";

    var div = this.wrapping = createHTMLElement("div");
    div.style.position = "relative";
    div.className = "CodeMirror-wrapping";
    div.style.width = options.width;
    div.style.height = (options.height == "dynamic") ? options.minHeight + "px" : options.height;
    // This is used by Editor.reroutePasteEvent
    var teHack = this.textareaHack = createHTMLElement("textarea");
    div.appendChild(teHack);
    teHack.style.position = "absolute";
    teHack.style.left = "-10000px";
    teHack.style.width = "10px";
    teHack.tabIndex = 100000;

    // Link back to this object, so that the editor can fetch options
    // and add a reference to itself.
    frame.CodeMirror = this;
    if (options.domain && internetExplorer) {
      this.html = frameHTML(options);
      frame.src = "javascript:(function(){document.open();" +
        (options.domain ? "document.domain=\"" + options.domain + "\";" : "") +
        "document.write(window.frameElement.CodeMirror.html);document.close();})()";
    }
    else {
      frame.src = "javascript:;";
    }

    if (place.appendChild) place.appendChild(div);
    else place(div);
    div.appendChild(frame);
    if (options.lineNumbers) this.lineNumbers = addLineNumberDiv(div, options.firstLineNumber);

    this.win = frame.contentWindow;
    if (!options.domain || !internetExplorer) {
      this.win.document.open();
      this.win.document.write(frameHTML(options));
      this.win.document.close();
    }
  }
```

- 然后我发现了代码, 作者把这部分代码分散在了editor.js里面, 作者确实是优先使用contenteditable的.
- 作者响应了下面三个事件
  - keydown
  - keypress
  - keup
- 但是, 看到这里我还是没有解决codemirror.replace是高阶函数, 他的参数是如何传入的问题. 

通过跟踪代码, 我知道了.

```js
//在jstest.html里面:
var editor = new MirrorFrame(CodeMirror.replace(textarea), {;//...
                                                           })
//这里第一个参数CodeMirror.replace(textarea)确实是一个函数

//然后在MirrorFrame里面, 这个函数得到了调用
function MirrorFrame(place, options) { //第二个参数只是给codemirror使用.
  this.home = document.createElement("div");
  if (place.appendChild) //place是第一个参数, 是codemirror给过来的. 现在去看看codemirror.
    place.appendChild(this.home);
  else
    place(this.home);//就是这里, 第一个参数作为函数被调用了.
//...
}
```

继续跟踪代码, 新的问题出现了, 系统逐个加载js, 可是, 这些js是注册再哪里的呢?

1. jstest.html里面注册了一个div. 这个div就是mirrorframe里面的this.home, 然后这个div被传给了codemirror.replace, 然后这些和frame其实都没有关系.
2. mirrorframe就是把本来页面上的textarea替换成了他自己的那一堆button.
3. mirrorframe初始化的最后一句: this.mirror = new CodeMirror(this.home, options); 这里和codemirror发生了关系.
4. codemirror闭包返回的其实是他自己的构造函数.
5. 有关系的是codemirror的初始化函数. 自动执行的闭包里面定义了自己的frame.
6. 然后用place(第一个参数)放置了codemirror制造的div, textarea, iframe. 而这个参数再mirrorframe调用的时候, 传入的是mirrorframe制造的那个包含很多button的div.

然后我们重新阅读codemirror构造函数:

```js
function CodeMirror(place, options) {
    // Use passed options, if any, to override defaults.
    this.options = options = options || {};
    setDefaults(options, CodeMirrorConfig);

    // Backward compatibility for deprecated options.
    if (options.dumbTabs) options.tabMode = "spaces";
    else if (options.normalTab) options.tabMode = "default";
    if (options.cursorActivity) options.onCursorActivity = options.cursorActivity; //这个是啥, 值得研究一下, todo

    var frame = this.frame = createHTMLElement("iframe");
    //制造iframe
    if (options.iframeClass) frame.className = options.iframeClass;
    frame.frameBorder = 0;
    frame.style.border = "0";
    frame.style.width = '100%';
    frame.style.height = '100%';
    // display: block occasionally suppresses some Firefox bugs, so we
    // always add it, redundant as it sounds.
    frame.style.display = "block";

    var div = this.wrapping = createHTMLElement("div");
    //看字面意思, 这事要做一个自动换行的div. 而且貌似是在iframe外边的.
    div.style.position = "relative";
    div.className = "CodeMirror-wrapping";
    div.style.width = options.width;
    div.style.height = (options.height == "dynamic") ? options.minHeight + "px" : options.height;
    // This is used by Editor.reroutePasteEvent
    var teHack = this.textareaHack = createHTMLElement("textarea");
    //这里有意思了, 又新建了一个textarea, 嘿嘿, 我把他显示出来看看.
    div.appendChild(teHack);
    teHack.style.position = "absolute";
    teHack.style.left = "100px";//原文这里是看不到的, 我改了一下.
    teHack.style.width = "100px";
    teHack.tabIndex = 100000;

    // Link back to this object, so that the editor can fetch options
    // and add a reference to itself.
    frame.CodeMirror = this;
    if (options.domain && internetExplorer) {
        this.html = frameHTML(options); //如果ie也会调这个代码.
        frame.src = "javascript:(function(){document.open();" +
            (options.domain ? "document.domain=\"" + options.domain + "\";" : "") +
            "document.write(window.frameElement.CodeMirror.html);document.close();})()";
    } else {
        frame.src = "javascript:;";
    }

    if (place.appendChild) place.appendChild(div);
    else place(div); //codemirror用第一个参数, 放置这个div. 
    div.appendChild(frame); //果然div里面放iframe.
    if (options.lineNumbers) this.lineNumbers = addLineNumberDiv(div, options.firstLineNumber);

    this.win = frame.contentWindow;
    if (!options.domain || !internetExplorer) {
        this.win.document.open();
        this.win.document.write(frameHTML(options));//不是ie也调这个.
        //貌似这个地方是关键, 继续研究一下
        this.win.document.close();
    }
}
```

研究内容:

1. onCursorActivity , 这个单独看貌似没啥意思, 等后面再碰到再说.
2. framehtml, 这个就是核心了, 我们一起来看一下.

排好队, 一个一个来.

```js
function frameHTML(options) {
    if (typeof options.parserfile == "string")
        options.parserfile = [options.parserfile];
    if (typeof options.basefiles == "string")
        options.basefiles = [options.basefiles];
    if (typeof options.stylesheet == "string")
        options.stylesheet = [options.stylesheet];

    var sp = " spellcheck=\"" + (options.disableSpellcheck ? "false" : "true") + "\"";
    var html = ["<!DOCTYPE HTML PUBLIC \"-//W3C//DTD HTML 4.0 Transitional//EN\" \"http://www.w3.org/TR/html4/loose.dtd\"><html" + sp + "><head>"];
    // Hack to work around a bunch of IE8-specific problems.
    html.push("<meta http-equiv=\"X-UA-Compatible\" content=\"IE=EmulateIE7\"/>");
    var queryStr = options.noScriptCaching ? "?nocache=" + new Date().getTime().toString(16) : "";
    forEach(options.stylesheet, function (file) {
        html.push("<link rel=\"stylesheet\" type=\"text/css\" href=\"" + file + queryStr + "\"/>");
    });
    forEach(options.basefiles.concat(options.parserfile), function (file) {
        if (!/^https?:/.test(file)) file = options.path + file;
        html.push("<script type=\"text/javascript\" src=\"" + file + queryStr + "\"><" + "/script>");
    });
    html.push("</head><body style=\"border-width: 0;\" class=\"editbox\"" + sp + "></body></html>");
    return html.join("");
}
```

然后, 我看到了这个:

```js
setDefaults(CodeMirrorConfig, {
    stylesheet: [],
    path: "",
    parserfile: [],
    basefiles: ["util.js", "stringstream.js", "select.js", "undo.js", "editor.js", "tokenize.js"],
    iframeClass: null,
    passDelay: 200,
    passTime: 50,
})
//初始的jstest.html里面也写了一点:
 parserfile: ["tokenizejavascript.js", "parsejavascript.js"],
 stylesheet: "css/jscolors.css",
```

然后, 这个iframe需要的js就都被引入了.

1. "util.js",  作者说这里是工具.
2. "stringstream.js",  这里是要用来parse的内容.
3. "select.js", 这里处理选中, 针对w3c和ie各写了一套.
4.  "undo.js", 这里处理undo
5.  "editor.js",  这个处理editor未来要细看.
6. "tokenize.js" 这个处理换行和空格这样的分隔符的, 需要细看.
7. "tokenizejavascript.js", 单独给javascript用的.
8. "parsejavascript.js" js的parser, 如果就是处理括号和引号, 那么其实不需要这个.

###### editor.js

这个看起来就很熟悉的感觉了. 

1. 处理空格折叠问题, 第二个空格开始nbsp

2. ……一路看下去.

3. keydown: 这里处理tab和回车.

4. keypress: 这里看是否要更新缩进.

5. keyup: 这里处理本行是否dirty(需要重新高亮).  处理6个光标按键: shift, control, alt, arrows, home,end

   

那么, 问题来了: delete和backspace什么时候处理?

- 都在keypress处理的. code分别为8和46.
- 还处理了code13 回车
- code9, 这个我记得是tab
- code32 空格

keydown处理了很多key

- 13
- 9
- 32
- 36 home
- 35 end
- 33 page up
- 34 page down
- 219和221 中括号
- 37 39 + meta  (左右)
- 回退, 保存, 粘贴, 都分别处理了.   

另外, 这里看到了: cursorActivity, 他会把某一行设置为dirty, 并且确保highlight执行.

> 成功没有侥幸, 这些代码令人肃然起敬, 作者就是用笨办法, 一个一个都处理了. 不禁让我想起tsp专家再[迷茫的旅行商]里面说的话, 2交换很简单, 5交换就要处理128种情况, 我们发现大神并没有回避, 而是真的逐一处理了这些情况, 最终拿出了一个最牛的方案.

至此code1主体阅读完毕. 未来需要细节我会再写续篇. 