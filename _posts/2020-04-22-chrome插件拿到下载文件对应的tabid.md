> 这个依旧求教育万能的Stack Overflow. 



### use chrome.webRequest api to monitor the download request, but tabid  is -1, how can i get correct tabid?



i want to build a chrome extension, download some file one by one, so i need pair the download file and the tabid that fire the download.

```
chrome.webRequest.onBeforeRequest.addListener(function (e) {
	log("onBeforeRequest:", JSON.stringify(e));
}, { urls: ["<all_urls>"] }, []);
```

use this code i can get the url, but can't get the tabid. the tabid is -1 allways, 
then, how can i get the correct tabid or how can i pair the download and the tabid?