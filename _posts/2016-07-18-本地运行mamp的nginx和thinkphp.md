1. apache模式可以直接运行.

2. nginx模式需要单独配置.  我单独配置了一个域名到本地. 然后需要从nginx.conf引入一个配置文件.

   ```sh
   #nginx.conf添加下面这句
   include /usr/local/etc/nginx/servers/*.conf; #这个地址要自己的目录.
   ```

   ```sh
   #引入的配置文件示例:
   server {
   	listen 80;
   	server_name test.hujing888.com;#ok.dev.me;
   	root /Users/m/Documents/work/erp;#/Users/dongzhenqi/www/oksir;
   	index index.html index.php;

   	location / {
   		if (!-e $request_filename)
   		{
   			rewrite ^/(.*)$ /index.php/$1  last;
   			break;
   		}
   	}
   	location /apidoc {
   		if (!-e $request_filename)
   		{
   			rewrite ^/apidoc/(.*)$ /apidoc/index.php/$1  last;
   		}
   	}
   	location ~ .+\.php($|/)
   	{
   		set $script    $uri;
   		set $path_info  "/";
   		if ($uri ~ "^(.+\.php)(/.+)") {
   			set $script     $1;
   			set $path_info  $2;
   		}
   		fastcgi_pass   127.0.0.1:9000;
   		fastcgi_index  index.php;
   		include fastcgi_params;
   		fastcgi_param PATH_INFO $path_info;
   		fastcgi_param SCRIPT_FILENAME  $document_root/$script;
   		fastcgi_param SCRIPT_NAME $script;
   	}
   }  
   ```

3. 然后, 页面访问错误从404, 403变成了502. google搜索, 发现是fastcg设置问题.

   ```sh
   #修改配置文件最后的部分. 贴入mamp默认的三句, 删除掉网上广泛流传, 但是已经时效(从别人机器复制)的三句.
   		fastcgi_pass     unix:/Applications/MAMP/Library/logs/fastcgi/nginxFastCGI.sock;
   		fastcgi_param    SCRIPT_FILENAME $document_root$fastcgi_script_name;
   		include          fastcgi_params;
   		
   		#fastcgi_pass   127.0.0.1:9000;
   		fastcgi_index  index.php;
   		#include fastcgi_params;
   		fastcgi_param PATH_INFO $path_info;		
   		# fastcgi_param SCRIPT_FILENAME  $document_root/$script;
   		fastcgi_param SCRIPT_NAME $script;
   ```

> 特别感谢, 
>
> captian技术支持.  上面的配置文件就是从captian处copy过来的.
>
> 温婉贤惠人生指引. 给了我写这份文档的可能性.

### 后记

补充一点, 搜索过程中看到很多比较靠谱的说法, 记录在这里, 未来或许我也能遇到这些坑.

>  李旭冲2013年12月03日
>
>  我也遇到过502 后来注释了 sql缓存队列 DB_SQL_BUILD_CACHE ;DB_SQL_BUILD_QUEUE ;DB_SQL_BUILD_LENGTH; DATA_CACHE_CHECK 后就正常了 可能是我的配置有问题



> 小狗快跑嘟嘟 5月1日发布
>
> nginx无论是用apt方式还是用编译方式都不麻烦，所以直接记录php-fpm的配置。
> 按照以前的思想，php-fpm工作在127.0.0.1:9000端口上，我直接配置后却出现了502 Bad GateWay页面。
> 查看nginx日志文件，发现错误是无法连接到本机9000端口。于是查找php-fpm.conf，在其兄弟文件夹pool.d下发现www.conf，关键一句打开是这样的listen = /var/run/php5-fpm.sock，通过查阅资料得知php-fpm有两种工作方式，一种是利用tcp，另一种直接使用socket，socket的方式会更快。因此，nginx下的配置fastcgi_pass 127.0.0.1:9000;的方式显然不支持socket方式。所以，只需要将其更改为fastcgi_pass unix:/var/run/php5-fpm.sock;以后，nginx就可以正常工作了。
>
> https://segmentfault.com/a/1190000005042321
>
> 对thinkphp的支持其实主要是对.htaccess文件的支持，主要是为了去掉index.php。实现有两种方式，其中一种是直接引入include $path/.htaccess，另一种是修改location / {}，但是apache和nginx的.htaccess文件语法不同，在nginx下正确的配置是
>
> ```sh
>    location / {
>    if (!-e $request_filename) {
>           rewrite  ^(.*)$  /index.php?s=$1  last;
>           break;
>    }
> ```

- 官网有标准写法: http://doc.thinkphp.cn/manual/hidden_index.html
- 官网的解决方案: http://www.thinkphp.cn/topic/3138.html
- http://www.sundabao.com/nginx下支持thinkphp的pathinfo和url-rewrite模式/
- http://my.oschina.net/zhuyajie/blog/523268
- 特殊思路, 不改nginx配置, 而是调整thinkphp思路: http://wpceo.com/thinkphp-nginx-configure-url-rewrite/