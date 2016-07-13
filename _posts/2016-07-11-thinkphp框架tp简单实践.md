### 第一个实践的框架 - thinkphp

#### 目录

初始目录:

1. index.php 入口文件
2. readme.md 简介
3. application 空目录
4. public 也是空目录
5. thinkphp框架目录

#### 参考手册吐槽

看了参考手册, 看的一头雾水, 感觉就是一个很复杂的框架. 然后, 看了快速入门, 才稍微有点感觉.

> 真心的, 以我粗浅的认知, 这是一个很傻的框架. 空的框架运行一下就发现了.

1. 他默认的竟然是php代码喷html代码的方式, 啥叫mvc啊???

2. 他采用了之前html开发里面最烦人的一点, image, css, html分目录存储, 这么干, 最终结果就是啥都不能删, 一切都很重要. 并且牵一发而动全身, 代码全都是互相关联, 最终冗余代码无数.

3. __font-family: "微软雅黑";__ 看到了吗? 用中文指定字体, 这个会被骂死, 因为这个字体的正确显示基于系统默认语言设置, 虽然用户有这个字体, 但是, 他依旧无法正常显示这个__Microsoft YaHei__字体. 这事和之前删除html的charset设置的人是一样的, 指望所有人都装__windows简体中文版__呢? 顺便说一句简体中文版才是问题所在.

   > 官方文档看这个: http://www.kancloud.cn/thinkphp/thinkphp_quickstart/2138
   >
   > 我一开始看错了文档, 看了3.1的, 和同事弄得有很大不同, 另外, 不建议看那个__完全参考手册__, 只见树木不见森林.

4. 自动生成代码, 唉, 我的同伴都是copy的好不好, 这个设计太脑残了, duplicate才是我们常用的模式好不好, 还自动生成, 真心有空哦, 直接就放一套模板数据不好么? 

#### 入门一半

1. 快速入门一半了, 都是没用的内容, 啥入口, 模块, 配置参数, 都没有用.

2. 总之 application/home/controller/indexcontroller.class.php这货是有用的, 是入口.

3. 然后在这个php里面的public方法都可以作为入口, 并且可以带参数访问, 我怀疑还是没啥用的特性.

   ```php
   //http://serverName/index.php/模块/控制器/操作
   //例如可以用这个url访问:  http://localhost:8888/index.php/home/index/hello/name/baby
   public function hello($name='thinkphp'){//只要这些代码放到indexcontroller.class.php, 上面的url就生效了.
          echo 'hello,'.$name.'!';
   }
   //这两个是等效的: 
   //http://serverName/
   //http://serverName/index.php/Home/Index/index
   ```

4. 然后又讲了url模式, 还是没啥用的功能, 就提供一种最好的就好了. 我们都选2-pathinfo, 这个模式会比较舒服, 比如:

   ```sh
   http://localhost:8888/taobao/getShopInfo    #taobaocontroller.class.php是默认模块的control. 
   #getshopinfo是他的方法函数.
   #每个方法函数, 在对应的control目录下都有一个view. 比如\view\taobao\getshopinfo
   ```

   ​

#### view

1. 关于view

   1. 其定义规范默认是模块目录下面的 View/控制器名/操作名.html，所以，Index模块的hello操作的默认模板文件位于Home模块目录下面的View/Index/hello.html

   2. 然后, 需要脱裤子放屁的弄这个:

      ```php
      public function hello($name='thinkphp'){
              $this->assign('name',$name);
              $this->display();
          }
      ```

   3. 然后可以浏览器访问试试.果然失败了, 失败原因是, 放屁之前还要再脱了胸罩

      ```html
      <html><!-- View/Index/hello.html -->
      <head>
        <title>hello {$name}</title>
      </head>
      <body>
          hello, {$name}!
      </body>
      </html>
      ```

   4. 再次有亲情提示, 上面的胸罩一定要放好, 放到view/index目录下, 不能和index.html放到一起, 佛祖啊. 而且还要注意大小写, 观音菩萨啊. 神经病啊, 首字母大写, 这个怎么搞, 难道不知道很多系统的默认首字母大写么? 这样弄别人怎么区分呢?

#### 数据库

1. 官方说配置在模块配置文件, 明显又想脱裤子, 我同事果断配置在公共配置文件, application/common/conf/config.php, 干得漂亮.

   ```php
      <?php
      return array(
      	//'配置项'=>'配置值'
      	'URL_MODEL'=>2,//2是去除index.php
      	'DB_FIELDTYPE_CHECK'   =>true,
          'TMPL_STRIP_SPACE'     =>true,
          'OUTPUT_ENCODE'        =>true, // 页面压缩输出
      	'MODULE_ALLOW_LIST'    =>    array('Home','Admin'),
          'DEFAULT_MODULE'       =>    'Home',  // 默认模块
      	//***********************************数据库设置****************************
      	'DB_TYPE'   => 'mysql', // 数据库类型
      	'DB_HOST'   => 'localhost', 
      	'DB_NAME'   => 'shock_g',
      	'DB_USER'   => 'root', //我的username
      	'DB_PWD'    => 'root', //我的password
      	'DB_PORT'   => 3306, // 端口
      	'DB_PREFIX' => 'g_', // 数据库表前缀 
      	'DB_CHARSET'=> 'utf8', // 字符集
      );
   ```

#### 实例

1. 这次回到index方法, 还是那个controller

   ```php
       public function index(){
         $Data     = M('reservation'); // 实例化我的表的数据模型, reservation是我的表名.
         $result     = $Data->select(); //find(1);
         $this->assign('result',$result);
         $this->display();        
       }
   ```

2. 回到/Users/m/Documents/thinkphp_3.2.3_full/Application/Home/View/Index/index.html

   ```html
   <html>
   <head>
     <title></title>
   </head>
   <body>
     <volist name="result" id="vo">
   	{$vo.phone}--{$vo.time}<br/>
     </volist>
   </body>
   </html>
   ```

#### 关于control

control的名字就是入口, 比如taobaocontroller.class.php, 入口就是http://xxx.ooo/taobao

### 关于路由

- tp5的路由规则讲的还是比较清晰的: http://www.kancloud.cn/thinkphp/thinkphp5_quickstart/145250
- http://serverName/index.php/模块/控制器/函数操作/参数名/实际参数值 
  1. 这里面index.php可以在设置模式为2的时候省略, 咱们一般都省略. 
  2. 模块名如果是默认模块也可以省略, 设置在common/conf/config.php
  3. 如果控制器是indexcontroller.class.php, 则可以省略这个控制器名.
  4. 如果方法是index, 则可以省略这个函数操作名
  5. 参数名和实际参数, 一般都是可以省略的.
  6. 模板地址: application/模块/view/controler名/controler函数方法名.html
- 因此http://servername/ 就可以访问: 
  1. 函数: 默认模块/controller/indexcontroler.class.php/index()函数.
  2. 模板: 默认模块/view/index/index.html

### 大写字母方法

http://www.thinkphp.cn/code/1985.html