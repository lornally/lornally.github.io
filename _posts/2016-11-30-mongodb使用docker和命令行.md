mongodb, 可以使用docker的.

1. 查看docker的接口.

   ```sh
   docker ps #查看docker的接口映射情况.
   ```


2. 修改docker里面, /根目录下面start.sh里面, 关于启动docker需要author的代码

3. 删除bootstrap/app.php里面的这个配置按照环境分配代码.

   ```php
   //根据不同环境加载不同配置
   $environment = getenv('DEV_ENV') ? '.' . getenv('DEV_ENV') : '';
   $app->loadEnvironmentFrom('.env' . $environment);
   ```

4. 测试的时候, 记得要关掉翻墙.

### 命令行

据说mongodb命令行才是王道. todo

### 客户端

1. MongoChef 速度飞快, 直接回显示内容数量, 必须是首选, 但是, 要记得不要选试用版, 选择免费版.
2. mongobooster 总体表现一般.
3. robomongo 还行.