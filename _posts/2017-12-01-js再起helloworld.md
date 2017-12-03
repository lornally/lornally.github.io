> 一切都回到起点, 20年之后, 发现自己还是要写js. 而js的领域已经很大了.

### 目标

在三个端写简单的markdown识别和展示功能. 目前的markdown对我而言不是太简单, 而是太复杂了, 有必要做一次瘦身了. 

1. js在html中处理.
2. js在本地处理.
3. js在编辑器中处理.

### 在html中处理js

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		*{
			font-family: "San Francisco Display", "Helvetica Neue", "Lucida Grande", "noto sans", Helvetica, Arial, sans-serif;
		}
	   html, body{/*此处必须用html, body处理, 不要用*号, *号是个大坑, 神奇的是, 每次都会掉进去*/
			height: 100%; 
			margin: 10px;
			padding: 10px;
		}
		textarea, p{
			height: 80%;
			margin: 10px;
			padding: 10px;	
		}
		
		textarea{
			font-size: 16px;
			width: 40%;
			float: left;
		}
		p{
			border: 1px solid green;
			float: left;
			width: 40%;	
		}
	</style>	
</head>
<body>
	<textarea class='left' >
### xxxxxx这里是段落, 每次输入的内容, 都会实时展现到右边.
	</textarea> 
	 <p class = 'right'>
		这里显示左边输入的内容.
	</p>	
	<script>
		var tex=document.querySelector('.left');
		var md=document.querySelector('.right');
		function sho(){
			md.innerHTML=tex.value;	
		}
	    tex.addEventListener('keyup', sho);
		 tex.focus();
	</script>
</body>
</html>
```

> 顺便吐槽下, 每次看到的helloworld代码, 都是没有屌用也没有卵用的猜数游戏. 