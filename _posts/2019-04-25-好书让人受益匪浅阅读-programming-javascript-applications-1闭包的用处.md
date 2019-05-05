> 偶然发现一本好书, 2015年的老书了. 中文版: javascript应用程序设计.

###### 作者开篇就震了我一下, 多少本书都没说清楚的事, 作者讲的很直白



原本的prototype方式: 这种方式很容易重复声明, 而且js还不报错:(

```js


var Lightbulb = function () {
	this.isOn = false;
},
	lightbulb = new Lightbulb();
Lightbulb.prototype.toggle = function () {
	this.isOn = !this.isOn;
	return this.isOn;
};
Lightbulb.prototype.getState = function getState() { // Implementation...
};
Lightbulb.prototype.off = function off() {
	// Implementation...
};
Lightbulb.prototype.on = function on() { // Implementation...
};
Lightbulb.prototype.blink = function blink() { // Implementation...
};
test('Prototypes without IIFE.', function () {
	equal(lightbulb.toggle(), true, 'Lightbulb turns on.');
	equal(lightbulb.toggle(), false, 'Lightbulb turns off.');
});

```

闭包iife方式

```js
(function () {
	var isOn = false,
		toggle = function toggle() {
			isOn = !isOn;
			return isOn;
		},
		getState = function getState() {
			// Implementation...
		},
		off = function off() {
			// Implementation...
		},
		on = function on() {
			// Implementation...
		},
		blink = function blink() {
			// Implementation...
		},
		lightbulb = {
			toggle: toggle,
			getState: getState,
			off: off,
			on: on,
			blink: blink
		};
	test('Prototypes with IIFE.', function () {
		equal(lightbulb.toggle(), true, 'Lightbulb turns on.');
		equal(lightbulb.toggle(), false, 'Lightbulb turns off.');
	});
}());

```

