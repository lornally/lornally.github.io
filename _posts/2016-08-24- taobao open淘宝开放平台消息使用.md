## 使用淘宝开放的消息接口分四步:

## 1 给用户开通消息 

> 这个函数其实只需要执行一次. 
> 这个函数需要token, 参见我之前的博文: 淘宝开放平台用户授权.

```php
/**
给用户开通消息
调用taobao.tmc.user.permit接口给用户（即淘宝或天猫商家）开通，可以选择只给用户开通部分消息类型，也可全部开通
	*/	
	//设置获取的消息内容
	$c = new TopClient;
	$c->appkey = $appkey;
	$c->secretKey = $secret;
	$req = new TmcUserPermitRequest;
	//$req->setTopics("taobao_trade_TradeCreate,taobao_refund_RefundCreate");
	$resp = $c->execute($req, $sessionKey);
	var_dump($resp);
```

## 2 得到订阅的消息列表. 
> 这是为了测试下是否成功的开通了消息.

```php

	$c = new TopClient;
	$c->appkey = $appkey;
	$c->secretKey = $secret;
	$req = new TmcUserGetRequest;
	$req->setFields("user_nick,topics,user_id,is_valid,created,modified");
	$req->setNick("testnick");
	$req->setUserPlatform("tbUIC");
	$resp = $c->execute($req);
	var_dump($resp);
	echo "<br>    hello  7.5  <br>  ";	
```

## 3 声明淘宝接口. 消费多条消息.

> 这个是主力函数, 淘宝建议写个死循环重复调用这个接口, 当然最好sleep几秒, 省流量, 并且免得淘宝觉得我们在攻击他们.

```php
	var_dump($appkey);
	print_r($appkey);
	$c = new TopClient; //这个地方必须\
	$c->appkey = $appkey;
	$c->secretKey = $secret;
	$req = new TmcMessagesConsumeRequest;
	$req->setGroupName("default");
	$req->setQuantity("100"); 
	echo "<br>    hello 8   <br>  ";
	#这里需要写个死循环, 不停地读取消息.
	$resp = $c->execute($req);


	echo "<br>    hello  9  <br>  ";
	var_dump($resp);
```

## 4 还要解析json.
```php
	foreach($resp->messages->tmc_message as $k=>$v){//resp不是json, $resp->messages->tmc_message才是json.
		$x=json_decode($v->content, TRUE, 512, JSON_BIGINT_AS_STRING); //这句话解析json为数组
		
      //下面两行是我的处理方法, 不必模仿. 反正已经拿到了json转为的数组, 所以可以随意处理.
      	if(handlem($x)) continue;// 如果有满足条件的项目, 那么进入下一轮.		
	 	if(is_array($x))showarray($x);	
	}
	
```

