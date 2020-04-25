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



大佬 [wOxxOm](https://stackoverflow.com/users/3959875/woxxom) 的答复

- This is a limitation of the API so you'll have to find a workaround
- I guess you'll have to intercept everything that may cause a download in your content script.
- It depends on how exactly that page starts the download. My idea was to use a standard DOM method like an event listener or intercept something used by the site in [page context](https://stackoverflow.com/a/9517879).
- Example: [Chrome extension to read HTTP response](https://stackoverflow.com/a/48134114) 

地址: https://stackoverflow.com/questions/61366130/use-chrome-webrequest-api-to-monitor-the-download-request-but-tabid-is-1-how?noredirect=1#comment108619365_61366130