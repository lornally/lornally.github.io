> 在nginx上面挑食thinkphp时, 这两个错误交替混杂出现, 解决了好久, 最后发现其实是小问题, 记在这里. 期望能够帮到同样遇到这个问题的同学. 

## thinkphp的报错: 无法加载控制器: xxx

> 如果url: xx.xxx.com/tbm/index.html

1. 如果是上面的URL, 那么__必须__设置url模式为2.

2. controller要求首字母__必须__大写, 

   - 比如TbmController.class.php 是正确的
   - tbmcontroller.class.php就是错误的.

3. cotroller里面的class__必须__和文件名一致.

   ```php
   namespace Erp\Controller; #必须声明namespace, 和目录结构一致.
   use Think\Controller; #必须引入基类.
   class TaobaomController extends Controller #这个类名必须和文件名一致
   ```

4. controller里面__必须__有相应的函数, 本例中函数必须是index(), 用来对应url尾部.

5. 对应的view目录下, __必须__有个目录Tbm(对应controller)

6. 目录里面__必须__有个文件, index.html, 对应cotroller的方法, 也对应url.

7. 如果还不行, 那么需要代码了:

   ```php
   public function __construct()
   	{
   		parent::__construct();
   		/**
   		卧槽这个是解决控制器找不到的办法.
   		Call to a member function assign() on null /ThinkPHP/Library/Think/Controller.class.php 　LINE: 122
   		*/
   		
   		Vendor('Taobao.TopSdk');
   	}
   ```

8. 这个框架用着真心__必须__累.

9. 其实还做了很多操作, 貌似没有用, 但是, 其实不能这么绝对, 列在下面, 如果上面这些都没有, 可以尝试下面这些: 

   - 重启服务器, 不行.
   - 删除runtime, 还是不行.
   - 修改nginx的fastcgi, 没有效果.
   - 清除chrome缓存
   - 切换服务器nginx<->apache

   ​



## nginx报错: 504

1. 一开始以为是设置问题, 检查各种设施.
   - 网上搜索也都说是设置问题.
2. 其实不是, 其实是使用了thinkphp之后, 不能用死循环去读取淘宝消息了. 
   - 因为死循环读取到淘宝消息, 导致的服务器崩溃. 唉. 估计是内存溢出了.
3. 解决方案: 不用thinkphp.
4. 其他尝试
   1. 清除chrome缓存
   2. 切换服务器nginx<->apache