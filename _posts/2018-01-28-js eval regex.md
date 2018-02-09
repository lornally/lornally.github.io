eval 或者说化妆过的eval, 比如electron的webcontent.executejavascript, 都会有问题

```js
//我滴神啊, 这个地方不是单引号, 是反引号, 我服了.....
const code=`const iframe=document.querySelector('iframe'); 
let content=iframe.textContent.replace(/\\u200C/mg, '\\n');
fs.writeFile('./xxx2.md', content, (err) => {
  if(err){
    log('An error ocurred creating the file '+ err.message);
  }
})`
win.webContents.executeJavaScript(code)
```

- webcontent 为毛要用反引号.
  - 明白了, 这个地方就是eval的问题了. 奶奶的果然学js不能漏过这么精华的内容...
  - 明白了为啥用反引号了, 因为多行, 一般的javascript字符串不支持多行. 奶奶的.
  - 因此, 我们需要做三件事之一
    - 行尾回车之前用\转义 --- 这个有点烦啊.
    - 链接字符串, 比如用+(加号)就可以 --- 这个也很烦啊.
    - 用反引号 --- 这个搞不定啊.
  - 因此放弃这种写法, 还是改回ipc写法吧. 这种写法太过于绕了, 坑太多.

关键字:

- eval
- regex
- Backticks backquote grave accent
- 依旧得不到啥有意义的信息. 彻底放弃....