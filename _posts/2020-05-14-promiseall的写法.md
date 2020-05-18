> 需要并行几个promise, 那么promiseall是唯一的选择

###### 写法一: 括号搞死人

```js
return Promise.all([
  (() => {
    if(!code) return;
    return inject("code",code);
  })(),
  ... (() => {
    if(!files) return [];
    return files.map(file => inject("file",file));
  })()
]);
```

###### 写法二: 声明了两个无用变量

```js
var c=code?inject("code",code):'no code';
var f=files?files.map(file => inject("file",file)): [];
return Promise.all([c,...f]);
```

###### 写法三: 上面两个结合一下

```js
return Promise.all([
  code ? inject("code",code) : 'no code',
  ...files ? files.map(file => inject("file",file)) : []
]);
```

