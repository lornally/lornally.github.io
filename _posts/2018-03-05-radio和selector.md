好有趣啊, 几乎完全一样的东西, 确是完全不同的处理方式.

- css选择器参考: https://developer.mozilla.org/zh-CN/docs/Web/CSS/:last-child
- radio参考: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio



###### radio和label

```html
<input type="radio" ... id="r1" /><label for="r1">button text</label>
用类似这样的选择器可以选到label: 
<style>
.radio-toolbar input[type="radio"]:checked+label {
</style>
or
<label for="r1"><input type="radio" ... id="r1" />button text</label>
```

- 一个radio成为一个group的条件是: 他们有同样的name.

###### 包含的元素除了用form

```
<fieldset> and <legend>
```

###### 选中的checkbox或者radio

```html
<script>
var gender = document.querySelector('input[name = "gender"]:checked').value;
</script>

<form id="rates">
  <input type="radio" name="rate" value="Fixed Rate"> Fixed
  <input type="radio" name="rate" value="Variable Rate"> Variable
  <input type="radio" name="rate" value="Multi Rate" checked> Multi
</form>
then...
<script>
var rate_value = rates.rate.value;
</script>
```

- onclick对于radio来说, 如果用键盘操作, 那么可能会不响应, 因此还是应该用addEventListener('change') 
- 如果是radio可以把事件放在外面的form上面: document.mainForm.onclick

> 最终结论, 很不幸, onclick写法和双事件写法都不行, 唯一可行的是: 				tdcode.addEventListener('change',updateradio), 示例如下: 

```html
<td id='tdcode'><h5>编码选择</h5>
    <input type="radio" name="code" id='code1' value="utf8" checked><label for="code1">utf8   </label>
    <input type="radio" name="code" id='code2' value="GB18030"><label for="code2">GB18030</label>
    <input type="radio" name="code" id='code3' value="EUC-CN"><label for="code3">EUC-CN    </label>
    <input type="radio" name="code" id='code4' value="Big5"><label for="code4">Big5</label>
</td>

<script>
const tdcode=document.getElementById("tdcode");
tdcode.addEventListener('change',updateradio)
</script>
  

下面这两种写法都是不被执行的:
//tdcode.onchage = updateradio
//tdcode.addEventListener('change, click',updateradio)
//tdcode.addEventListener('change click',updateradio)
```



###### 选中selector

```html
<select id="ddlViewBy">
  <option value="1">test1</option>
  <option value="2" selected="selected">test2</option>
  <option value="3">test3</option>
</select>

Running this code:
<script>
var e = document.getElementById("ddlViewBy");
var strUser = e.options[e.selectedIndex].value;
Would make strUser be 2. If what you actually want is test2, then do this:

var e = document.getElementById("ddlViewBy");
var strUser = e.options[e.selectedIndex].text;

用onchange做监听
```





###### 顶对齐

```
display: inline-block;
vertical-align: top;
```

参考: https://developer.mozilla.org/en-US/docs/Web/CSS/vertical-align



###### document selector

- queryselector返回第一个满足条件的element
- parentNode.querySelectorAll返回一个nodelist
  - 实际使用中还是用document.querySelectorall
  - 或者element.queryselectorall, 比如一个selector
- RadioNodeList 
  - RadioNodeList.value 这个就是checked radio button

```html
<script>
var form = document.forms[0];
var radios = form.elements['myRadio'];//radionodelist
console.log(radios.value);
radios.value = 'Mr.';
var myForm = query('form#myForm');
var myRadioList = myForm.queryAll('input[name=radioName]'); //radianodelist
myRadioList.value; // == first checked item
</script>
<input type=radio name=my_radio value=value_1>
<script>
var my_form = document.querySelector('#my_form');
var my_radio = my_form.elements.my_radio; //radionodelist
监听事件放到form上面
```

