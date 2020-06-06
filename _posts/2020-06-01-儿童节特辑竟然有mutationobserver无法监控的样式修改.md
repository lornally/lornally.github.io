> 很奇怪的一个现象: 页面修改了, 但是, 代码监控不到.




### [can we change element's color, but not invoke mutationobserver?](https://stackoverflow.com/questions/62120062/can-we-change-elements-color-but-not-invoke-mutationobserver)

Are there some page changes (such as colors) that cannot be monitored by mutationobserver in chrome? because i have write a chrome extension, that monitor the page by mutationobserver, but i find document.body's background-color has been change, i find it by my eye, not by code,

in-fact document.body.computedStyleMap().get('background-color').toString() and getComputedStyle(document.body).getPropertyValue('background-color') is not changed ( that background-color is old value still),

but in devtools the computed style is changed by css (that background-color is new value). so strange, does anyone have any idea about it?



大神 [wOxxOm](https://stackoverflow.com/users/3959875/woxxom)给出了答案: 

- MutationObserver won't see changes made via CSS API like insertRule and so on. 

并且还给了方法: 

- You'll have to spoof all its methods in [page context](https://stackoverflow.com/a/9517879). 确认大神的意思是: inject code change the CSSAPI's prototype
- BTW maybe your case is simpler, use MutationObserver to detect `<style>` elements added or changed in entire `document` recursively.