先说connect, 看代码就知道了

> 官方文档虽好, 然而内容太多了: https://www.npmjs.com/package/connect
> 因此, 还是推荐大家用stackoverflow解决问题.

```js
//const con=require('connect')


//connect 大改版了, 因此, 语法变了.
const connect = require('connect'),
    serveStatic = require('serve-static');

const app = connect();

app.use(serveStatic("./web"));
app.listen(5000);


//下面是改了一次的写法
// var app = con();
//
// const se=require('http').createServer()//con.createServer()
//
//
// se.use(con.static(__dirname+'/web'))
// se.listen(3000)
//
//

//手贱, 最原始的写法被我删掉了. 总之smashing node 里面的代码已经跑不起来了. 从stackoverflow copy了别人的
var connect = require('connect');
connect.createServer(connect.static("./web")).listen( 5000);


// const fs=require('fs')
//
// require('http').createServer(
// 	(req, res) => {
// 		res.writeHead(200, {'Content-Type': 'text/html'})
//
// 		se(__dirname+'/index.html')
// 		//res.end("hello xxxxxxxxxxxxxx")
//
// 		function se(path){
// 			fs.createReadStream(path).pipe(res)
// 		}
// 	}
// ).listen(3000)


```

再说request

```js
	request.get(url,  { qs: x },	httpr)	
```

必须要说的是, 今天是2018年1月21日, 上面的代码是可以运行的. 