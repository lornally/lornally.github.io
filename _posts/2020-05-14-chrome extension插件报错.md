> chrome插件开发中不可避免的有插件报错, 但是chrome是抽象派的朦胧诗, 报错报的非常销魂

1. the message port closed before a response was received

   - 这个错的意思是: 你没写response.

   - 我回答了这个问题: https://stackoverflow.com/questions/54736574/how-to-implement-return-true-error-the-message-port-closed-before-a-respon/61795822#61795822

     ```
     return true can resolve your problem just because: 
     
     you always need to sendresponse, you can sendresponse to any value, but you must sendresponse.
     
     In your code, there are many situations, than no sendresponse.
     
     This sendresponse function becomes invalid when the event listener returns, unless you return true from the event listener to indicate you wish to send a response asynchronously (this will keep the message channel open to the other end until sendResponse is called).
     
     ref: https://developer.chrome.com/extensions/runtime#event-onMessage
     ```

   - 如果你暂时无法resqonse, 那么你可以return true, 变成异步的.

   - https://stackoverflow.com/questions/20077487/chrome-extension-message-passing-response-not-sent/61796169#61796169

     ```
     eturn true make the sendresponse is asynchronous, but it keep the message channel open. so maybe you can just sendresponse to resolve it.
     
     always sendresponse, i suggest it.
     return true, maybe is not a good practice.
     ```

   - https://stackoverflow.com/questions/54017163/how-to-avoid-the-message-port-closed-before-a-response-was-received-error-when

