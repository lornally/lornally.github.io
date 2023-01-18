> range并没有形成规范, 但是, webkit有本身的实现

## 用css的var实现

> 参考: https://stackoverflow.com/questions/62681404/change-css-property-of-range-slider-thumb-via-js-jquery更好的参考: https://insidethediv.com/select-css-pseudo-element-using-javascript

```html
<label for="cow">奶量input range webkit伪类</label>
<input type="range" id="cow" name="cow" min="0" max="100" value="5" step="1">
<input type="button" value="30" id="m30" name="m30">

```

```sass
input[type=range]
    -webkit-appearance: none
    --boxAfterFontSize: 20px
    // 下面是更好的写法
    width: var(--rangewidth,800px)
    background: red
    //此处&不可省略, 不然会是有空格的结果, 有空格, 代表对于子元素的描述
    &::-webkit-slider-runnable-track 
        // width: 50% 这个属性不生效
        height: 5px
        background: #acc
        border: none
        border-radius: 3px
    &::-webkit-slider-thumb 
        -webkit-appearance: none
        border: none
        height: 16px
        width:  var(--boxAfterFontSize)
        border-radius: 2px//50%
        background: goldenrod
        margin-top: -4px
    &:focus, &:hover
        outline: none
        // width: 500px
        // background: green

        &::-webkit-slider-runnable-track 
            background: #ccc
```

```coffeescript
c30c= (e)->
 cow.style.setProperty('--boxAfterFontSize','80px')
 cow.style.setProperty('--rangewidth','400px')
m30.addEventListener "click",c30c
```
## 增加一个cssrule, 并且修改他

> 参考: https://stackoverflow.com/questions/311052/setting-css-pseudo-class-rules-from-javascript

```coffeescript
# 插入一条rule
c50c= (e)->
 ruler="
 #cat::-webkit-slider-thumb{
  background: pink;
  width: 30px;
 }"
 document.styleSheets[0].insertRule(ruler, 0) 
 # 在位置0插入一条css, 更合理的玩法是插到尾部, 保证同等级生效
m50.addEventListener "click",c50c
# 修改一条rule
i=60
c10c= (e)->
 document.styleSheets[0].cssRules[0].style.backgroundColor= 'red'
 document.styleSheets[0].cssRules[0].style.width= "#{i+=i}px"
m10.addEventListener "click",c10c
```





## 不推荐的硬插写法

```js
ar styleElem = document.head.appendChild(document.createElement("style"))
styleElem.innerHTML = "#theDiv:before {background: black;}"
```

## 参考

- https://www.quirksmode.org/blog/archives/2015/11/styling_and_scr.html
- https://developer.mozilla.org/en-US/docs/Web/CSS/::-webkit-slider-thumb
- https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes
