###### 入门

这个东西是基于promise的. 先学习下promise吧.

- promise就是为了链式调用弄得语法糖.

```js
doSomething() 
.then(result => doSomethingElse(value)) //这里只处理正确的事, result是前面dosomething的返回结果.
.then(newResult => doThirdThing(newResult))
.then(finalResult => console.log(`Got the final result: ${finalResult}`))
.catch(failureCallback); //上面不论谁有bug, 这里都会catch住.
```

- async/await可以更整齐

```js
async function foo() {
  try {
    let result = await doSomething();
    let newResult = await doSomethingElse(result); //而且更直观
    let finalResult = await doThirdThing(newResult);
    console.log(`Got the final result: ${finalResult}`);
  } catch(error) {
    failureCallback(error);
  }
}
```



###### 事件响应

- 我也想异步, 但是onchange这样的写法太方便了. 没有必要啊. 而且这个地方也不会抛出异常.
- 结论: 就正常写法吧, 或许哪一天我能发现这个地方可以改进.

###### 读文件开始已经应该是异步了.



###### fetch 

12-27的blog写了fetch的写法.链式调用的写法: 

```js
//更好地写法, 避免括号地狱
fetch(url).then(function(response) {
  return response.text()
}).then(function(text) {
  poemDisplay.textContent = text;
});
```

看看怎么改成async写法, 这样写果然更规整.

```js
 try {
        const fileContents = await readjsonfile(files[0]); //拿到文件
        const response = await fetch('http://hztch.wdy.com/', {
          method: 'POST', // or 'PUT'
          body: fileContents, // data can be `string` or {object}!
          headers: new Headers({
            'Content-Type': 'application/json'
          })
        }); //ajax拿结果
        const showdata = await response.json();
        log(showdata)
      } catch (e) {
        console.warn(e.message);
      }
```



###### promise包装了filereader

```js
/**
     * 包装promise async版本filereader
     */
const readjsonfile = (inputFile) => {
    const reader = new FileReader();
    return new Promise((resolve, reject) => {
        reader.onerror = () => {
            reader.abort();
            reject(new DOMException("Problem parsing input file."));
        };
        reader.onload = () => {
            resolve(reader.result);
        };
        reader.readAsText(inputFile); //从FileReader.readAsBinaryString() 改过来.
    });
};
```



###### 存在一个反模式

参考: https://stackoverflow.com/questions/23803743/what-is-the-explicit-promise-construction-antipattern-and-how-do-i-avoid-it

- 错误写法1:

```js
function getStuffDone(param) {           
    var d = Q.defer(); /* or $q.defer */ 
    // or = new $.Deferred() etc.            
    myPromiseFn(param+1)                       
        .then(function(val) { /* or .done */          
        d.resolve(val);                               
    }).catch(function(err) { /* .fail */          
        d.reject(err);                                
    });                                       
    return d.promise; /* or promise() */    
}                                         
```

- 错误写法2:

```js
function getStuffDone(param){
    return new Promise(function(resolve, reject) {
        myPromiseFn(param+1)
            .then(function(val) {
            resolve(val);
        }).catch(function(err) {
            reject(err);
        });
    });
}
```

正确写法:

```js
function getStuffDone(param){
    return myPromiseFn(param+1); // much nicer, right?
}
```



###### 如何转化代码为promise

- 参考这个: https://stackoverflow.com/questions/22519784/how-do-i-convert-an-existing-callback-api-to-promises



###### 更多参考

- 还有6篇文档需要阅读, 有空的时候再读.





