> Stack Overflow提问环节又到了

### [why backgroundimage is not cssImageValue in chrome? how can i use canvas deal with background-image but not make a new image?](https://stackoverflow.com/questions/61981581/why-backgroundimage-is-not-cssimagevalue-in-chrome-how-can-i-use-canvas-deal-wi)

base on this article:  https://developer.mozilla.org/en-US/docs/Web/API/CSSImageValue

```js
const allComputedStyles = button.computedStyleMap(); 
// Return the CSSImageValue Example 
console.log( allComputedStyles.get('background-image') );
```

but in chrome:

```js
var img=imgelement.computedStyleMap().get('background-image')
canvas.getContext('2d').drawImage(img,0,0,img.width,img.height);
//output: Uncaught TypeError: Failed to execute 'drawImage' on 'CanvasRenderingContext2D'
//: The provided value is not of type '(CSSImageValue ....
```

I want to use canvas check a background-picture is black, but in chrome, how can i do it.

btw, I didnot want to build a new image tag with the background image's url. please give me a more direct way. 

### 震惊

神奇的事情出现了, 竟然忽然就好了, 返回的就是cssimagevalue了. 也就是说: chrome 修了一个bug?





last update, finally, i  found out the truth, because there are more than one value with background-image:

```css
background-image: linear-gradient(rgba(0, 0, 0, 0.65), rgba(0, 0, 0, 0.65)), url("http://localhost:8080/icons/we-flow.png"); //here is cssstylevalue

background-image: url("http://localhost:8080/icons/we-flow.png"); //here is cssimagevalue
```





---
now 5.29 2020 the bug back, chrome  83.0.4103.61, 
now it is cssstylevalue not cssimagevalue who know why?

-----

now 5.26 2020, @Kaiido find chrome fix this bug, now, we can get cssimagevalue :

    var img=element.computedStyleMap().get('background-image')