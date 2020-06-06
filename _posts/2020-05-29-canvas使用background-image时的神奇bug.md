> 有问题问Stack Overflow

```
canvas.drawImage with computedStyleMap().get("background-image"), DOMException: Failed to execute 'getImageData' on 'CanvasRenderingContext2D': The canvas has been tainted by cross-origin data.
```



```html
<div id='img3'>background-image with this</div>
```

```css
#img3 {
  background-image: url("../icons/we-flow.png");
}
```

```js
var img = img3.computedStyleMap().get("background-image");
//img=img1;
var canvas=document.createElement('canvas');
img.crossOrigin="Anonymous";
var ctx=canvas.getContext('2d');
try {
  ctx.drawImage(img, 0, 0);
  var data=ctx.getImageData(0, 0, canvas.width, canvas.height).data;
}catch (error) {
  console.log('refresh page will get this', img.toString(), {error, img});
}
```

run this code with chrome 83, first time load the code, everything is ok, but when i refresh the page in browser, i will get the console.log. my question is why? an how can i fix it?



经过一天的分析, 又重新提了一个问题, 这个问题终于惊动了大神: [wOxxOm](https://stackoverflow.com/users/3959875/woxxom)

In development, I need to judge the color of the picture. Therefore, canvas is used, but it will face cross-domain problems when processing, 

so I am solving it by modifying the http header, but this method is useful for image tags, but for background-image, It will be invalid, 

do you have any good solutions? 

I know that extracting the background-image url, and then creating a new image should solve this problem, 

I would like to ask if there is a better and more convenient way?


In html
```html
<div id='imgwiki'>this is wrong, because use background-image</div>

<img id="imgb" src="https://ssl.gstatic.com/images/icons/material/system_gm/2x/content_copy_black_24dp.png"	alt="imgb"> this is allways ok. because it is a img tag

```


In my content.js

```js
var img=document.getElementById("imgwiki");
img=img.computedStyleMap().get("background-image");
img.crossOrigin="Anonymous";
var canvas=document.createElement('canvas');
var ctx=canvas.getContext('2d');
ctx.drawImage(img,0,0);
var data=ctx.getImageData(0,0,canvas.width,canvas.height).data; //here cross-domain
```

In my background.js
```js
chrome.webRequest.onHeadersReceived.addListener(
	function(details) {
		luoism.log({details}, details.url);
		return {responseHeaders: [...details.responseHeaders,{name: "Access-Control-Allow-Origin",value: "*"}]};
	},
	{urls: ['<all_urls>']},   // filters
	['blocking','responseHeaders','extraHeaders']   // extraInfoSpec
);
```


@wOxxOm, actually this code is ok, but i want a more directly solution:
```js

		!async function getcssimage(e) {

			var stylestr=e.computedStyleMap().get('background-image').toString();

			//get url
			var url='',ur=/url\("([^)"]+)/,x=false;
			if(!ur.test(stylestr)) return false;
			url=ur.exec(stylestr);
			url=url[1];

			//deal mix http & https 
			url=luoism.config.ishttps? url.replace(/^https*/,'https'):url;

			//from background-image url to <img> tag
			function makeimg(url) {
				return new Promise(resolve => {
					let img=new Image();
					img.addEventListener("load",e => {
						resolve(e.currentTarget);
					});
					img.onerror=e => {
						let t=e.currentTarget;
						t.onerror="";
						t.style.display='none';
						e.stopPropagation();
						e.preventDefault();
						resolve(false);
					};
					img.crossOrigin="anonymous";
					img.src=url; 
				});
			}


     // deal this <img>, anything is ok
			var img=await makeimg(url);
			var canvas=document.createElement('canvas');
			var ctx=canvas.getContext('2d');
			luoism.log('消息begin',img.toString(),{img});
			ctx.drawImage(img,0,0);
			var data=ctx.getImageData(0,0,canvas.width,canvas.height).data;
			return x;
		}(img);
```