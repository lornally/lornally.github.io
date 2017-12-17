> 需要对业务中最重要的接口做拉力测试, 简单的说, 就是一个5000*5000的循环扫描.

### 总结: 访问http

- 复杂的原生方法

```javascript
//复杂的方法
var url = 'http://graph.facebook.com/517267866/?fields=picture';

http.get(url, function(res){
    var body = '';

    res.on('data', function(chunk){
        body += chunk;
    });

    res.on('end', function(){
        var fbResponse = JSON.parse(body);
        console.log("Got a response: ", fbResponse.picture);
    });
}).on('error', function(e){
      console.log("Got an error: ", e);
});

```

- request方法

```javascript
var request = require('request') //http处理库request
const url='http://trainapi.vip.elong.com/home/getStartEndStation'
    //http访问的参数, 这里要参数化.
const restring={ 'startDate': date, 
    'startStation': star,
    'endStation': end,
}
request.get(url,  { qs: restring },	httpr(error, response, body))
//request.get request.post
//参数可以指定json 或者qs
//一句话写成下面这样.
request.get({url: "http://localhost:3000/my-api-controller", 
             qs: {url: url}},
            function(error, response, body){
               console.log(body);
            });
// 加一个超时设置: timeout.
```

- post json

```javascript
var request = require('request');

request.post(
    'http://www.yoursite.com/formpage',
    { json: { key: 'value' } },
    function (error, response, body) {
        if (!error && response.statusCode == 200) {
            console.log(body)
        }
    }
);
```

- 加上管道啥的

```javascript
//最简单的
var request = require('request');
request('http://www.google.com', function (error, response, body) {
  console.log('error:', error); // Print the error if one occurred
  console.log('statusCode:', response && response.statusCode); // Print the response status code if a response was received
  console.log('body:', body); // Print the HTML for the Google homepage.
});

//管道到文件
request('http://google.com/doodle.png').pipe(fs.createWriteStream('doodle.png'))
//文件到http
fs.createReadStream('file.json').pipe(request.put('http://mysite.com/obj.json'))
//文件和管道互相
request.get('http://google.com/img.png').pipe(request.put('http://mysite.com/img.png'))
```



### 参考

- https://www.twilio.com/blog/2015/10/asyncawait-the-hero-javascript-deserved.html