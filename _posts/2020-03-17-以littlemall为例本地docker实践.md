> 最近有朋友创业, 想要搭个电商的小平台. 和几个后端朋友商量了一下, 决定使用littlemall

###### 尝试的平台

- git@github.com:linlinjava/litemall.git
- 为什么是它: 排名高并且比较简单, 项目处在初始阶段, 分工还不够细, 因此2-3个人应该是可以维护起来的. 太大规模的电商项目就不是一两个人能维护的了.

###### 路径

- 我负责前端. 并且非常不希望开发环境把我的机器搞崩.
- 因此, 我们用docker, 具体的安装参见近日的两篇隔壁文档: 本地开发... 和docker

###### 总体思考

- 第一步: 数据库
- 第二步: 后端运转起来
- 第三步: 前端运转起来

既然这样, 我能不能合成一步, 找一个image包含以下内容:

- mysql
- java环境
- maven
- nodejs

答案是, 这样并不好, 浪费了docker的优势, docker的优势就是灵活方便, 这样既不灵活, 也不方便. 某个软件升级就可能搞死整个container.

dockerfile和compose

###### mysql

```sh
$ docker run --name my-s -d -p 3306:3306 -v /Volumes/wd4black/mysql -e MYSQL_ROOT_PASSWORD=root mysql
#特别重要这里的--name不能省略, 不然会报错"docker run" requires at least 1 argument.
```

解释

```sh
--name         后面是这个镜像的名称
-p 3306:3306   表示在这个容器中使用3306端口(第二个)映射到本机的端口号也为3306(第一个)
-d             表示使用守护进程运行，即服务挂在后台
-e             设置环境变量
-v             本地的路径 
```

- 然后就可以发现sequel-pro无法连接, 网上查了一下, 竟然是这个软件的问题. 推荐软件dbeaver.
- 然后dbeaver报另一个错误:  Unable to load authentication plugin 'caching_sha2_password'. Unable to load authentication plugin 'caching_sha2_password'., 原因是要在新建连接的时候, 搜索mysql, 找到mysql8.
- 这货下载非常慢, 需要配置proxy, 注意, 他只支持http
- 然后这货报新的错误: Public Key Retrieval is not allowed, 这个错误堪称一言难尽. 参考Andre Tzermias的答案, 这个答案的执行都几乎可以说是大家来找茬, 太难了, 做IT还要考验观察力 : 
  - https://community.atlassian.com/t5/Confluence-questions/MySQL-Public-Key-Retrieval-is-not-allowed/qaq-p/778956
  - https://stackoverflow.com/questions/50379839/connection-java-mysql-public-key-retrieval-is-not-allowed

还有另一个思路, 使用老的认证方式

```sh
docker run --name my-s -d -p 3306:3306 -v /Volumes/wd4black/mysql -e MYSQL_ROOT_PASSWORD=root mysql --default_authentication_plugin=mysql_native_password
```

不过在导入sql的时候, 报一堆错误, 因此尝试下mysql5

```sh
$ docker run --name mysql5 -d -p 3305:3306 -v /Volumes/wd4black/mysql -e MYSQL_ROOT_PASSWORD=root mysql:5.7.29
```

果然换了mysql5之后, 一切都平静了!  (￣o￣).zZ    彡(-_-;)彡   o((⊙﹏⊙))o.   

----------------------------------------------------------------------------------------------------------------------------

这里最重点的就是用: mysql5!!!!!!!!!!!!!!!    (´థ౪థ）σ       乂(ﾟДﾟ三ﾟДﾟ)乂 

------------------------

顺便说一句, 就在下的观察, 实事求是的讲dbeaver就是一个垃圾, mysql也是.......

###### java/spring

- 貌似需要docker-compose
- 看官网的三份文档:
  - https://spring.io/guides/gs/spring-boot-docker/
  - https://spring.io/guides/topicals/spring-boot-docker/
  - https://docs.docker.com/compose/
- 还有两份最佳实践文档: 
  - https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
  - https://docs.docker.com/develop/dev-best-practices/
- 还有案例, 依照之前chrome extension的经验案例恐怕是最重要的.
  - https://docs.docker.com/samples/
  - https://github.com/docker/labs/tree/master/developer-tools/java/
  - https://github.com/play-with-docker/play-with-docker.github.io
- 思路
  - 应该不是从Ubuntu开始.而是从java开始.

