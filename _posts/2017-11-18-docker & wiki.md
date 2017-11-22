### 缘起

1. gitlab的wiki有一个致命的问题, 太难做条目链接.
2. wikimedia有一个致命的问题, 难于备份.

docker + wiki解决了这个问题, 一个镜像走天下. 



### 入门之前的步骤

```sh
## 第一步 如果没有安装docker, 那么需要安装docker, 我同时安装了docker 和cask下面的docker
brew install docker
brew cask install docker #这之后还可以选择安装kitematic
## 第二步 拿到mediawiki镜像
docker pull mediawiki
```

> kitmatic的尝试失败了

直接启动了docker

但是, 此时发现配置wiki的时候, 需要数据库的映射.......

回到命令行

```sh
docker images #看看是否下载了mediawiki, 其实没下载也没关系, docker会自动下载images(镜像)
docker run -d -p 80:80 --name xxx nginx
#这句话的意思是: 运行一个镜像 主机端口(例如80, 任意设置的):镜像内部端口(例如80镜像固定的) 名字是 代号(例如xxx, 任意设置的) 内部名(例如nginx, 镜像固定的)
#此时如果没有nginx这个镜像, docker会自动去下载这个镜像
docker ps #查看docker的镜像的运行情况, 会展示端口映射情况.
```

### 入门一下 - 第一部分 制造一个镜像container

> 闹明白究竟是啥情况

1. 只要一个目录里面有dockerfile这个文件, docker就可以把这个目录转化为docker镜像.
2. 然后就可以运行这个镜像. 这个镜像里面如果有个python或者nodejs就可以直接跑起来, 并不需要在镜像里面装python环境. 因为docker会自动搞定这些.

文件示例: 

> dockerfile

```dockerfile
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

> 因为dockerfile指定了: `app.py` and `requirements.txt`. 因此这两文件也得有. 先上txt

```
Flask
Redis
```

> py文件: 

```python
from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>" \
           "<b>Hostname:</b> {hostname}<br/>" \
           "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

> 然后, 在这三个文件存在的基础上, 我们可以搞一个docker镜像.

```sh
docker build -t friendlyhello . #编译本地.目录
docker run -p 4000:80 friendlyhello #运行这个镜像
```



> 如何把弄好的镜像传给别人

1. 打个tag, 这是一个可选项. 不打也行.
2. 发布这个tag
3. 如果没有打tag, 那么自动打latest

```sh
docker push username/repository:tag #上传一个镜像, 这里的tag是可选的
docker run -p 4000:80 username/repository:tag #这时候, 如果没有这个镜像, 就会去远程拉一个
```

> 最基础的命令行列表

```sh
docker build -t friendlyname .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyname  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyname         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
```

### 入门一下 第二部分 - 指定镜像的运行方式service

建立一个docker-compose.yml文件

```yaml
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag #拿到某一个镜像
    deploy:
      replicas: 5  #运行五个实例
      resources:
        limits:
          cpus: "0.1"  #每个实例占用10%的cpu
          memory: 50M  #占50m内存
      restart_policy:
        condition: on-failure #如果失败则立刻重启服务
    ports:
      - "80:80"  #映射80端口
    networks:
      - webnet  #负载均衡的网络名为webnet
networks:
  webnet:
```

然后开始我们的命令行操作

```sh
docker swarm init #准备工作
docker stack deploy -c docker-compose.yml getstartedlab #这里的getstartedlab是自己任意命名的, 就是给这个负载均衡服务起个名字.
docker service ls #这时能看到前面命名的getstartedlab
docker service ps getstartedlab_web #进一步看到里面的5个实例
docker container ls -q #列出容器id, 这个id和下面命令行/浏览器看到的id是一致的
curl -4 http://localhost #或者在浏览器中刷新 查看id. 这个和上一个是匹配的.
#当你修改了docker-compose.yml文件, 就需要重新部署一下
docker stack deploy -c docker-compose.yml getstartedlab
#关闭这个负载均衡app
docker stack rm getstartedlab
docker swarm leave --force #关掉swarm
```

附上命令行介绍

```sh
docker stack ls                                            # List stacks or apps
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
docker service ls                 # List running services associated with an app
docker service ps <service>                  # List tasks associated with an app
docker inspect <task or container>                   # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                             # Tear down an application
docker swarm leave --force      # Take down a single node swarm from the manager
```

### 入门的第三部分集群两层负载均衡swarm

swarm是一个集群cluster, 其中每个机器是一个接单nodes. docker可以运行在swarm mode.

原文在这里: https://docs.docker.com/get-started/part4/#create-a-cluster

这个真的进入到了配置管理和运维的领域, 可有趣了, 组成多层的负载均衡

可以配置多个docker-machine, docker-machine用虚拟机或者物理机都可以.

### 入门的第四部分 栈集成 stacks

stacks是一个集成环境, 运行一整个服务, 哈哈, wiki应该在这里了.

> 特大喜讯, 1. wiki就在这个领域, 2. 咱们其实在之前已经用过栈了, docker stack deploy

继续处理docker-compose.yml

```yaml
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      restart_policy:
        condition: on-failure
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
    ports:
      - "80:80"
    networks:
      - webnet
  visualizer: #另一个service
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    volumes: #
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]
    networks:
      - webnet
networks:
  webnet:
```

> 部署过程因为需要模拟多个机器, 因此用到了上一章的内容, 原文参考: https://docs.docker.com/get-started/part5/#add-a-new-service-and-redeploy

如果我们增加一个redis存储, 依旧是修改docker-compose.yaml

```yaml
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      restart_policy:
        condition: on-failure
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
    ports:
      - "80:80"
    networks:
      - webnet
  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]
    networks:
      - webnet
  redis:
    image: redis
    ports:
      - "6379:6379"
    volumes:
      - /home/docker/data:/data
    deploy:
      placement:
        constraints: [node.role == manager]
    command: redis-server --appendonly yes
    networks:
      - webnet
networks:
  webnet:
```

看到这里明白了, 之前的小伙伴用命令行启动的时候直接指定了目录对应关系.

docker目前把这些映射关系的逻辑, 都从命令行转移到了yaml文件里面.

### 入门第五部分 部署

这一部分就是把docker放到云端, 哦了, 公司用的ocean, 总之给为小伙伴各找各妈, 看看如何部署到云端吧.

原文以amazon的aws举例: https://docs.docker.com/get-started/part6/#deploy-your-app-on-a-cloud-provider

### 再一次领悟

mediawiki只是php, 不包含他需要的各种服务, 比如mysql等等, 因此, 他的docker也是这样的, 需要指定mysql或者其他db. 所以, 我们应该使用包工具弄这个. 比如xampp.

一步安装: mediawiki-stack stack栈就是整个包的意思.



### 再次尝试

```sh
docker run --name miki --link msql:mysql -d mediawiki 
#果然报错, 提示没有mysql
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
#some-mysql is the name you want to assign to your container, my-secret-pw is the password to be set for the MySQL root user and tag is the tag specifying the MySQL version you want 所以我们用下面这句
docker run --name msql -e MYSQL_ROOT_PASSWORD=mckmck -d mysql
#另一个程序使用mysql:
docker run --name some-app --link some-mysql:mysql -d application-that-uses-mysql
#所以就是一开始那句话, 现在再次执行一下试试
#启动没啥问题, 但是, 无法访问localhost, 忽然想起, 苹果默认不开放80端口. 所以还是要用8080
#先停止mediawiki 
docker stop miki
#然后使用加上了端口映射的这一句:
docker run --name miki -p 8080:80 --link msql:mysql -d mediawiki 
#然后docker说container重名了, 前面那个根本就不能用, 当然要移除. 咋移除呢?
docker rm -f miki #这个益处了miki这个container. 
docker images #查看镜像
docker rmi nginx #移除镜像nginx
docker ps -a #能看到所有的container
```

### 然后开始浏览器配置

1. 数据库的主机名要用刚才设置的docker容器名: msql, root密码要用刚才设置的mckmck
2. wiki管理员密码要8位以上.
3. 然后我们发现无法copy文件进去, 咋办? 做个目录映射

```sh
#再加上目录映射, 先映射根目录试试. 
docker run --name miki -p 8080:80 -v /Users/machangkun/Documents/X/docker:/ --link msql:mysql -d mediawiki 
#先把msql那个container起来
docker start msql
#然后发现根目录还不允许映射. /war/www/html /etc/pki/tls 除了-v还用了-h, 主要是有-it
#/var/www/html 我快哭了, 原来是拼写错误, 不是war 是var

docker run --name miki -p 8080:80 -v /Users/machangkun/Documents/X/docker:/var/www/html --link msql:mysql -d mediawiki 

#然后还是报错, 貌似要用--volume? 先建立一个mediawiki_data, 再用--volume来用它.
#我去, 懂了, 网上的做法都是弄一个debian或者ubuntu再装wiki

#然后, 正解再这里:
docker run --name <your_wiki_name>  --link mediawiki-db:postgres  -p 8080:80 \  
-v <wiki_root>/data/rw:/data/rw \ #<wiki-root>就是本地目录 ~/Documents/X/docker
-v <wiki_root>/images:/usr/share/nginx/html/images \
-v <wiki_root>/extensions:/tmp/extensions \
-v <wiki_root>/LocalSettings.php:/usr/share/nginx/html/LocalSettings.php \ #这里就是刚刚弄好的localsettings
-d simplyintricate/mediawiki

docker run --name miki  --link msql:mysql  -p 8080:80 -v /Users/machangkun/Documents/X/docker/data/rw:/data/rw -v /Users/machangkun/Documents/X/docker/images:/usr/share/nginx/html/images -v /Users/machangkun/Documents/X/docker/extensions:/tmp/extensions -v /Users/machangkun/Documents/X/docker/LocalSettings.php:/usr/share/nginx/html/LocalSettings.php -d mediawiki

### 竟然不行, 再试试不用~, 我试试download, 因为dockment受到icloud控制

/Users/machangkun/Downloads


docker run --name miki  --link msql:mysql  -p 8080:80 -v /Users/machangkun/Downloads/docker/data/rw:/data/rw -v /Users/machangkun/Downloads/docker/images:/usr/share/nginx/html/images -v /Users/machangkun/Downloads/docker/extensions:/tmp/extensions -v /Users/machangkun/Downloads/docker/LocalSettings.php:/usr/share/nginx/html/LocalSettings.php -d mediawiki

### 试试这一句:
docker run --name miki  --link msql:mysql  -p 8080:80  -v /Users/machangkun/Downloads/docker/LocalSettings.php:/var/www/html/LocalSettings.php -d mediawiki


#官方说了, 还要这个: /var/www/html/images

docker run --name miki  --link msql:mysql  -p 8080:80  -v /Users/machangkun/Downloads/docker/images:/var/www/html/images -v /Users/machangkun/Downloads/docker/LocalSettings.php:/var/www/html/LocalSettings.php -d mediawiki


# 再加上logo, 从localsetting看到了: /resources/assets/wiki.png
docker run --name miki  --link msql:mysql  -p 8080:80  -v /Users/machangkun/Downloads/docker/logo.png:/var/www/html/resources/assets/wiki.png -v /Users/machangkun/Downloads/docker/images:/var/www/html/images -v /Users/machangkun/Downloads/docker/LocalSettings.php:/var/www/html/LocalSettings.php -d mediawiki

# 再试试, 是否能够不映射上传目录出来? 因为默认是不设置为可以上传文件的, 我改了, 新的localsetting是可以上传的, 那么我们不映射上传路径出来应该是可以运行的吧? 试验下, 这个如果可以, 我们的服务器部署就比较简单了.

docker run --name miki  --link msql:mysql  -p 8080:80  -v /Users/machangkun/Downloads/docker/logo.png:/var/www/html/resources/assets/wiki.png  -v /Users/machangkun/Downloads/docker/LocalSettings.php:/var/www/html/LocalSettings.php -d mediawiki




```



