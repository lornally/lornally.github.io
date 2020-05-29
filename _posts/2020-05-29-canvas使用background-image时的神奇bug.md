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