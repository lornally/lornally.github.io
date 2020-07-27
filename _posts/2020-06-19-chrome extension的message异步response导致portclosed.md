> 如果在listener里面异步, 那么会遇到一个bug: [“The message port closed before a response was received”](https://stackoverflow.com/questions/54017163/how-to-avoid-the-message-port-closed-before-a-response-was-received-error-when)

- 感谢woxxom

```js
/**
 * 接受消息
 */
chrome.runtime.onMessage.addListener(function(message,sender,sendResponse) {
	luolib.log('后台响应消息:',JSON.stringify(message));
	switch(message.type) {
		//如果是判断图片的消息
		case luoconf.messagetype.blackpicture:
			//const x=await luolib.picinfo(message.url);
			(async () => {
				sendResponse(await luolib.picinfo(message.url));
			})();
			//sendResponse('收到了');
			// 			sendResponse((await luolib.picinfo(message.url)).isblack);
			return true;
	}
});

```

- https://stackoverflow.com/questions/54017163/how-to-avoid-the-message-port-closed-before-a-response-was-received-error-when
- 要点:
  - listener不能是async, 但是, 可以用IIFE
  - return true; 确实有用, 前提是listener不能是async.
  - 既然这个可以, 那么问题就转变为, 后台的这些数据需要缓存一下. 