now 2020, and i find the really solution, get style and set style must use different method: 

```js
function changeColor(cell) {
  var red = "rgba(255, 4, 10, 1)"; //rgba or rgb, so if you really want to use this, you need use regexp. 
  var grey = "rgba(230, 230, 230, 1)"; //pls change your css to this too.


  var style = cell.computedStyleMap().get('background-color').toString();

  if (style == grey) {
    cell.style.backgroundColor = red;
  } else  if (style == red) {
    cell.style.backgroundColor = grey; 
  };

```

for chrome this is ok, but, maybe other browser with other color format, you need test it.



另一个问题中解释了hsl在html中的使用.

if you want to set as hsl, just like this format: 

    divElement.style.backgroundColor = "hsl(155,100%,30%)";

but you want to got the style, that must be rgb or rgba string. so you must change it to hsl by your self.



you have two method get the live style of a dom element(not css), but you can not get hsl, but only rgb/rgba string.

```js
var style = window.getComputedStyle(element).getPropertyValue('background-color')

var style = cell.computedStyleMap().get('background-color').toString();
```


i think this is really quick
```js
document.querySelectorAll('body,body *').forEach(function(e) {
```


you must chang to the hsl format just like:  "hsl(155,100%,30%)", that can be use in html.
```js
//this change hsl to :  "hsl(155,100%,30%)"
function hsl2str({h,s,l,a=1}) {
	return "hsl("+h+","+s*100+"%,"+l*100+"%,"+a+")";
}
//for html, the h is 0-360, so you need this function
function rgb2hsl({r,g,b,a=1}) {
	r/=255,g/=255,b/=255;
	var max=Math.max(r,g,b),min=Math.min(r,g,b);
	var h,s,l=(max+min)/2;

	if(max==min) {
		h=s=0; // achromatic
	} else {
		var d=max-min;
		s=l>0.5? d/(2-max-min):d/(max+min);
		switch(max) {
			case r: h=(g-b)/d+(g<b? 6:0); break;
			case g: h=(b-r)/d+2; break;
			case b: h=(r-g)/d+4; break;
		}
	}

	return {h: h*60,s,l,a};
}


```