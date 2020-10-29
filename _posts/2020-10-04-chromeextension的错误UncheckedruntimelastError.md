任意的 Unchecked runtime.lastError 都是一个原因: 插入js或者css是, 没有check error. 

解决: 在回调函数中checkerror就好了.



例如: 

```js
//插入css
function injectcsscode({tabid,code,allFrames=true}) {
  chrome.tabs.insertCSS(tabid,{code,allFrames},e=>checklasterror('动态注入css code失败'+e));
}
/**
 * 检查lasterror
 */
function checklasterror(a) {
  chrome.runtime.lastError? log(JSON.stringify(a),'lasterror',chrome.runtime.lastError):'';
  return chrome.runtime.lastError;
}
```

