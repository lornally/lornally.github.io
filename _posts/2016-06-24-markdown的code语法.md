> 神奇了, 我之前只知道两种code语法, 今天发现还有第三种. 很帅的.

1. 用反引号`, 比如下面, 如果用三个反引号可以指定code类型.

   ```js
   // Example can be run directly in your JavaScript console

   // Create a function that takes two arguments and returns the sum of those arguments
   var adder = new Function("a", "b", "return a + b");

   // Call the function
   adder(2, 6);
   // > 8
   ```

2. 用⇥或者空格.

   	// Example can be run directly in your JavaScript console

   	// Create a function that takes two arguments and returns the sum of those arguments
   	var adder = new Function("a", "b", "return a + b");
   	
   	// Call the function
   	adder(2, 6);
   	// > 8

3. 但是发现了吗? 上面两个格式, 代码都是没有颜色的, 如果要颜色咋办? 下面这种格式 __{   % highlight js %   }__ 这种格式就ok了.
   {% highlight js %}
   // Example can be run directly in your JavaScript console

   // Create a function that takes two arguments and returns the sum of those arguments
   var adder = new Function("a", "b", "return a + b");

   // Call the function
   adder(2, 6);
   // > 8
   {% endhighlight %}

4. 但是,  上面这个格式, 没有可以成功review的编辑工具, 不论macdown还是typora.

5. dao了这里, 我发现自己错了. 第一段三个反引号的写法真心是更好的写法, 颜色都一样的. 忍不住好奇, 再试试一个引号的写法:

   `// Example can be run directly in your JavaScript console`

   `// Create a function that takes two arguments and returns the sum of those arguments`
   `var adder = new Function("a", "b", "return a + b");`

   `// Call the function`
   `adder(2, 6);`
   `// > 8`
   ​

6. 明白了, 单个反引号, 只用引用一行.

#### 到了这里, 真相大白了. 多行代码, 还是用三个反引号, 最靠谱.