> JavaScript就是js, 多年之后, 发现JavaScript已经不是从前, 这只证明了一件事, 这货不够优秀, 至少优秀程度不如C, 正则, markdown, git, wiki, lisp…….

```javascript
 ( function ($参数) { 
   //这里是代码
 } )(实际传进去的参数);//也就是说定义一个匿名函数, 然后立马执行他.
```

### jquery

为啥要弄这么原始的东西呢? 因为, 我在研究马蜂窝, 他们用这个东西加载页面内容, 研究发现1.12的源码是带注释的. 奶奶的, 新的源码没有注释了, 貌似.

```javascript
jQuery.ready.promise = function( obj ) {
  if ( !readyList ) {
    readyList = jQuery.Deferred();
    // Catch cases where $(document).ready() is called
    // after the browser event has already occurred.
    // we once tried to use readyState "interactive" here,
    // but it caused issues like the one
    // discovered by ChrisS here:
    // http://bugs.jquery.com/ticket/12282#comment:15
    if ( document.readyState === "complete" ) {// Handle it asynchronously to allow scripts the opportunity to delay ready
      window.setTimeout( jQuery.ready );
      // Standards-based browsers support DOMContentLoaded
    } else if ( document.addEventListener ) {
      // Use the handy event callback
      document.addEventListener( "DOMContentLoaded", completed );
      // A fallback to window.onload, that will always work
      window.addEventListener( "load", completed );
      // If IE event model is used
    } else {// Ensure firing before onload, maybe late but safe also for iframes
      document.attachEvent( "onreadystatechange", completed );
      // A fallback to window.onload, that will always work
      window.attachEvent( "onload", completed );
      // If IE and not a frame
      // continually check to see if the document is ready
      var top = false;
      try {
        top = window.frameElement == null && document.documentElement;
      } catch ( e ) {}
      if ( top && top.doScroll ) {
        ( function doScrollCheck() {
          if ( !jQuery.isReady ) {
            try { 
              // Use the trick by Diego Perini
              // http://javascript.nwbox.com/IEContentLoaded/
              top.doScroll( "left" );
            } catch ( e ) {
              return window.setTimeout( doScrollCheck, 50 );
            }
            // detach all dom ready events
            detach();
            // and execute any waiting functions
            jQuery.ready();
          }
        } )();
      }
    }
  }
  return readyList.promise( obj );
 };
```



参考:

1. http://sunnylost.com/article/jquery.ready.html
2. http://www.cnblogs.com/giggle/p/5246798.html
3. http://rapheal.sinaapp.com/2013/01/30/jquery-src-ready/
4. http://rapheal.sinaapp.com/2013/01/17/jquery-src-util/
5. 最后两篇是一个系列, 很不错, 剖析jquery, 等我弄js的时候, 要去研究.



### chrome分析

1. 用inspect 定位页面元素. 发现元素的id或者class.
2. 全局搜索id和class定位到JavaScript.
3. 发现加载的ajax文件.
4. 到network查看router.php, 下面可以看到各种header和form data. (这些参数是有用的, 要copy到postman)
5. 可以看到下面这些JavaScript了.

```javascript
<script type="text/javascript"> //代码质量真心不咋地.
(function ($) {var tagId, page = 1;
$('.nav li').click(function () {
  var $this = $(this);
  if(!$this.hasClass('on')){
    tagId = $this.data('tagid');
    page = 1;
    getPage($this); }}).eq(0).click();
function getPage($element) {$.ajax({
  url: '/ajax/router.php',  dataType: "json",
  data: {
    'sAct': 'KMdd_StructWebAjax|GetPoisByTag',
    'iMddid': 10183,
    'iTagId': tagId || 0,
    'iPage': page
  },  type: 'post',
  success: function (ret) {
  if (ret.succ) {
$element && $element.addClass('on').siblings().removeClass('on');
$('.row-allScenic .scenic-list').html(ret.data.list);
$('.row-allScenic ._j_tn_pagination').html(ret.data.page).find( 'a[data-page]').click(function () { page = $(this).data('page');
  getPage();});}}});}})(jQuery);
```

### chrome分析补遗

1. source这里可以设置各种断点
   1. JavaScript异常.
   2. 代码的某一行. 还可以设置条件断点.“Edit breakpoint”, 比如for循环中的断点, 我们只想看到某些情况的执行.
   3. 某种类的事件, 比如鼠标单击
2. 主页面一般就是第一个source.
3. source中的代码都是可以直接修改的. 
4. console可以输出日志.
5. console可以运行脚本.
6. elements可以调样式.
   1. break on可以监听. 貌似这个地方设置断点更合理.
7. workspace可以直接改, 直接看效果, 直接保存源码.


### chrome调试的参考

1. http://wiki.jikexueyuan.com/project/chrome-devtools/debugging-javascript.html
2. http://han.guokai.blog.163.com/blog/static/136718271201321402514114/
3. https://segmentfault.com/a/1190000000431586
4. http://colinued.leanote.com/post/%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91%E7%A5%9E%E4%B8%80%E6%A0%B7%E7%9A%84%E5%B7%A5%E5%85%B7chrome%E8%B0%83%E8%AF%95%E6%8A%80%E5%B7%A7
5. https://www.zhihu.com/question/20260762


### chrome可以

- ctrl+o 打开一个js文件
- ctrl+p 同ctrl+o
- ctrl+f 查找当前js文件中的关键字
- ctrl+shift+f 全局查找关键字
- ctrl+shift+e 在控制台运行当前选中的代码片段

### postman

1. headers 里面只需设置cookie

2. body里面四个参数, 这些都填充上面chrome开发工具看到的那些.

3. postman报错还是比较准确的, 他说哪个参数错了, 基本上就是那个参数错了.

4. 参数值不需要转义转码.

5. 使用预执行script(pre-request)

   ```javascript
   document.cookie="PHPSESSID=t8efu16md1t6m1cir2o9l68df7; mfw_uuid=5840e43c-5e80-5c9e-05d0-92f90856c93c; __mfwurd=a%3A3%3A%7Bs%3A6%3A%22f_time%22%3Bi%3A1480647746%3Bs%3A9%3A%22f_rdomain%22%3Bs%3A0%3A%22%22%3Bs%3A6%3A%22f_host%22%3Bs%3A3%3A%22www%22%3B%7D; __mfwuuid=5840e43c-5e80-5c9e-05d0-92f90856c93c; oad_n=a%3A3%3A%7Bs%3A3%3A%22oid%22%3Bi%3A1029%3Bs%3A2%3A%22dm%22%3Bs%3A15%3A%22www.mafengwo.cn%22%3Bs%3A2%3A%22ft%22%3Bs%3A19%3A%222016-12-15+17%3A31%3A57%22%3B%7D; __mfwlv=1482201913; __mfwvn=8; CNZZDATA30065558=cnzz_eid%3D612852601-1480644536-%26ntime%3D1482200023; uva=a%3A4%3A%7Bs%3A2%3A%22lt%22%3Bi%3A1482203297%3Bs%3A10%3A%22last_refer%22%3Bs%3A6%3A%22direct%22%3Bs%3A5%3A%22rhost%22%3Bs%3A0%3A%22%22%3Bs%3A4%3A%22step%22%3Bi%3A17%3B%7D; __mfwlt=1482203307";
   //然后, postman就报错了. 他不能执行document. 神奇了.
   ```

   ​

### 采集

1. 不能再使用DOMDocument->LoadHTMLFile
2. 尝试下file/file_get_content/
3. 前面的判断貌似错了, 貌似context就是设置这个的, 
4. 和curl的抉择有点费心思.

### 回顾

1. 使用file/file_get_content/可以配合正则perl regex
2. 使用DOMDocument->LoadHTMLFile配合xpath
3. 使用curl配合xpath

### 对比

```php
// using file_get_contents to submit a support ticket 
$post_array = array (
  "email" => "someone@gmail.com",
  "problem" => "error happened on baes...bla bla bla"
);
$post_string = http_build_query($post_array);
$opts = array( //参考: http://php.net/manual/zh/wrappers.php
  'http' => array( //参考: http://php.net/manual/zh/context.http.php
    'method' => "POST",
    'header' => "Content-Type: application/x-www-form-urlencoded",
    'content'=> $post_string
  )
);
// http://php.net/manual/zh/function.stream-context-set-option.php
// http://php.net/manual/zh/function.stream-context-create.php
$context = stream_context_create($opts);
// http://php.net/manual/zh/function.file-get-contents.php
// https://doc.phpspider.org/development_skills/file_get_contents-proxy.html
$result = file_get_contents('http://www.cubebackup.com/ticketsubmit.php', false, $context);
if ($result === FALSE) //...
```

```php
// using CURL lib to do the same job
$post_array = array (
  "email" => "someone@gmail.com",
  "problem" => "error happened on bacs...bla bla bla"
);
$post_data = http_build_query($post_array);//这句未必需要.
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'http://www.cubebackup.com/ticketsubmit.php');
// in real life you should use something like:
// curl_setopt($ch, CURLOPT_POSTFIELDS, 
//          http_build_query(array('postvar1' => 'value1')));

// receive server response ...
//curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);

curl_setopt($ch, CURLOPT_POST, TRUE);
curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);

//curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
$res = curl_exec($ch);
curl_close($ch);
if ($res === FALSE) //...
////////////////////////////////////
$curl = new Curl\Curl();
$curl->put('http://api.example.com/user/', array(
    'first_name' => 'Zach',
    'last_name' => 'Borboa',
));
```

```php
 //PECL_HTTP implemention
 $problemDesc="error happend on bacs... bla bla bla";
 $post_string = "email=someone@gmail.com&problem=".$problemDesc;
$res = http_post_data('http://www.cubebackup.com/ticketsubmit.php', $post_string); 
if ($res === FALSE) { // ... }
```

### 对比总结

1. curl依旧是最好的. 类似正则
2. 如果简单可以考虑, 类似各种字符串函数, 但是, 稍微复杂一点点比如post, 那么这个方法就要求我们记住很多常量和content结构. 而且他还比较快. 因为做了很多优化, 比如缓存dns.
3. pecl扩展明显更优雅. 话说正则的扩展也是pecl, 其实就是在curl外边包了一层. 据说这个扩展需要安装, 并且安装后还要编译php源码.......

### 看考

1. 关于性能: http://www.programering.com/a/MzMxETNwATI.html
2. 性能原文: https://mdb9.wordpress.com/2011/03/06/file_get_contents-vs-curl-what-has-better-performance/
3. 入门参考: https://www.codeproject.com/tips/1019822/comparison-of-the-http-libs-in-php-file-get-conten
4. 简单封装: https://www.fusionswift.com/2010/02/curl-vs-file_get_contents/

### 简单封装

```php
<?php
function curl_get_contents($url) {
       // Initiate the curl session
 $ch = curl_init();
      // Set the URL
 curl_setopt($ch, CURLOPT_URL, $url);// Removes the headers from the output
 curl_setopt($ch, CURLOPT_HEADER, 0);// Return the output instead of displaying it directly
 curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);// Execute the curl session
 $output = curl_exec($ch);
    // Close the curl session
 curl_close($ch);
    // Return the output as a variable
 return $output;
}
?>
```

### 优化curl

```php
观察发现init和close都比较费时间, 是否能一次次init, 多次使用呢?
stackoverflow说可以的
$ch = curl_init();
while(true){
  这里搞exec
}
curl_close($ch);
```

### 扒取的参数

```javascript
1. 图片.
var mddid = 172703, poiid = 7518;
2. 页面.
data: {
  'sAct'   : 'KMdd_StructWebAjax|GetPoisByTag',
  'iMddid' : 10183,
  'iTagId' : tagId || 0,
  'iPage'  : page
},
```

