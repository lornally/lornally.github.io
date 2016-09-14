> 很多淘宝接口是需要授权的, 怎么拿到授权呢?

### 拿到code/token等等授权. 

> 后文中, 所有__你的xxx__都是在淘宝开放平台的控制台可以直接看到的.

1. 拿到code, 其实很简单, 不需要php, 妹的, 一句url在浏览器跑一下就好了. 妹的.

   ```sh
   https://oauth.taobao.com/authorize?response_type=code&client_id=你的appkey&redirect_uri=http://test.你的回调地址.com/  #就是这样的url就好了.
   #然后看地址栏, 就可以拿到code了.
   ```

2. 拿到token, 依旧不需要php, 应该用curl去弄. 阿弥陀佛. 善哉善哉.

   ```sh
   curl -i -d "code=8vRdC-你的code-715654&grant_type=authorization_code&client_id=233你的key839&client_secret=a9429你的安全码d2a529e&redirect_uri=http://test.你的回调地址.com/" https://oauth.taobao.com/token #执行这一段代码. 其中最重要的是code要随着上面的结果而变化.
   ```

3. token结果示例, 也不需要php的json解析, 眼见就可以拿出来.

   ```json
   {"taobao_user_nick":"%E4返回nick%97","re_expires_in":0,"expires_in":86400,"expire_time":1471104542733,"r1_expires_in":400,"w2_valid":14733,"w2_expires_in":800,
    "taobao_user_id":"2473729354","w1_expires_in":400,"r1_valid":14542733,"r2_valid":1472733,"w1_valid":1472733,"r2_expires_in":400,"token_type":"Bearer",
    "refresh_token":"62027216a3987b80c053刷新用token在这里, 但是, 其实没卵用, 因为刷新接口, 在我测试的时候还没有调好. 4dfhe1786088adb57a402473729354",
    "refresh_token_valid_time":1471018142733,
    "access_token":"6293这里会有好长的token, 这个token就是你要的54"} #这最后一个项就是token.
   ```

4. 消息如何设定为json? 很简单. 

   ```php
   $c = new TopClient;
   $c->appkey = $appkey;
   $c->secretKey = $secret;
   $c->format='json';//这一句就是指定返回格式为json.
   $req = new TmcMessagesConsumeRequest;
   $resp = $c->execute($req);
   ```

   ​

### 之前的笨办法, 用php搞, 真够笨的. 极度不建议大家使用, 太自虐了.

授权比较复杂, 下面有拆解.

```php
$c = new \TopClient(); # 在tp中必须加 \ 因为调用了淘宝sdk
 $output = $c->curl($url, $postField); # 淘宝api直接传数组进去, 可以.
 $outputArr = json_decode($output, true); #json解析一下.
 $req = new TradesSoldGetRequest; #某个接口需求.
# ...$req需要设置的参数....
 $resp = $c->execute($req,$this->accessToken); #调起这个接口.
```

如果已经有了token. 那么:

```php
 $c = new TopClient; # 声明一个cliend
 $req = new TradesSoldGetRequest; #某个接口需求.
# ...$req需要设置的参数....通常有appkey啥的.
 $resp = $c->execute($req,$this->accessToken); #调起这个接口. 如果不要token, 则不需要第二个参数.
```

如果没有token, 但是有授权code

```php
  $url = "https://oauth.taobao.com/token";
      $postField = array(
        'grant_type'=>'authorization_code',
        'client_id'=>$this->appKey,
        'client_secret'=>$this->secret,
        'code'=>$code,
        'redirect_uri'=>$this->redirectUri
      );
      $c = new \TopClient(); #thinkPHP里面 必须加 \ 因为调用了淘宝sdk
      $output = $c->curl($url, $postField); //淘宝的接口, 所以直接传了数组进去.
      $outputArr = json_decode($output, true);     
      $this->accessToken = $outputArr['access_token']; // 把token存起来.
```

如果code都没有, 那么需要先拿到code:

```php
#比较简洁的写法, 推荐.
$url = 'https://oauth.taobao.com/authorize?response_type=code&client_id='.
$this->appKey.'&redirect_uri='.$this->redirectUri;
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL,$url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER,1);
$output = curl_exec($ch);   

#或者这么写: 
$url = 'https://oauth.taobao.com/authorize';
$postfields= array('grant_type'=>'authorization_code',
                   'client_id'=>$appkey, //这个地方appkey
                   'client_secret'=>$secret, //这个地方secret
                   'code'=>'test', //这个地方是前面传过来的code
                   'redirect_uri'=>'http://test.hujing888.com', //这个是回调地址.
                   'response_type'=>'code'	//不指定这个会报错.
                  ); 
// 1. 初始化
$ch = curl_init();
// 2. 设置选项，包括URL
curl_setopt($ch,CURLOPT_HEADER,0);
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
//curl_setopt ($ch, CURLOPT_SSL_VERIFYPEER, 0);
//curl_setopt ($ch, CURLOPT_SSL_VERIFYHOST, 0);
//指定post数据
curl_setopt($ch, CURLOPT_POST, true);
//添加变量
curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($postfields));// substr($post_data,0,-1));
// 3. 执行并获取HTML文档内容
$output = curl_exec($ch);
$httpStatusCode = curl_getinfo($ch, CURLINFO_HTTP_CODE); # 得到code. 
```

