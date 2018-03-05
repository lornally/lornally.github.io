好有趣啊, 几乎完全一样的东西, 确是完全不同的处理方式.





###### radio和label

```html
<input type="radio" ... id="r1" /><label for="r1">button text</label>
用类似这样的选择器可以选到label: 
.radio-toolbar input[type="radio"]:checked+label {
or

<label for="r1"><input type="radio" ... id="r1" />button text</label>
```

###### 包含的元素除了用form

```
<fieldset> and <legend>
```





###### 顶对齐

```
display: inline-block;
vertical-align: top;
```

参考: https://developer.mozilla.org/en-US/docs/Web/CSS/vertical-align

