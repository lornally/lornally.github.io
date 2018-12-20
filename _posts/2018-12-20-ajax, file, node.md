###### 客户端:

file拖入

```js
//dropbox是任意的页面元素, 比如div
dropbox.addEventListener("drop", drop, false);

function drop(e) {
    e.stopPropagation();
    e.preventDefault();

    var dt = e.dataTransfer;
    var files = dt.files;

    handlefiles(files);
  }
```

 读本地文件

```js
let reader = new FileReader();
reader.readAsBinaryString(files[0]);
//回调函数, 这里这样写也可以: reader.onload = function (f) {... }
reader.onload = (function (f) {  
    return function (e) {
        f(e.target.result);
    };
})(sendpara); //sendpara就是回调函数, 作为参数(f)穿进去
```

ajax

```js
//发参数
let r = new XMLHttpRequest();
r.open("POST", '/', true); //访问根目录
r.setRequestHeader("Content-Type", "application/json");
r.send(txt); //发送请求

//拿返回结果
 r.onreadystatechange = function () {
     if (this.readyState === XMLHttpRequest.DONE && this.status === 200) {
         dealresult(JSON.parse(this.responseText)); //处理返回结果
     }
 }
```

###### 后台node

```js
require('http').createServer(function (req, res) {
   //处理浏览器的get请求
   if ('GET' == req.method) {
       // 处理js
       if (req.url.substr(0, 5) == '/show' && '.js' == req.url.substr(-3)) {
           serve(__dirname + req.url, 'application/javascript');
           return;
       } 

       // 处理图片
       if (req.url.substr(0, 5) == '/show' && '.png' == req.url.substr(-4)) {
           serve(__dirname + req.url, 'application/png');
           return;
       } 

       // 处理网页
       if (req.url.substr(0, 5) == '/test' && '.html' == req.url.substr(-5)) {
           serve(__dirname + req.url, 'text/html');
           return;
       } 
       // 处理index.html
       serve(__dirname + '/show/index.html', 'text/html');
       // log('default:', req.url)
       return;
   }
    function serve(path, type) {
        res.writeHead(200, {
            'Content-Type': type
        });
        fs.createReadStream(path).on('error', function (err) {
            log("err:", err);
            return;
        }).pipe(res)
    }
    /**
     * 正常处理正常接口调用post
     */
    let body = ''; //这个不能拿到外边去, 这个不能作为全局变量处理
    req.on('data', function (c) {
        body += c;
    })
    req.on('end', function () {
        initarray(body, res); //生成输出数组
    })
}).listen(8008)
```

