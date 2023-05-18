## Docker镜像命令


```
### 帮助命令

docker version  # 显示 Docker 版本信息。
docker info    # 显示 Docker 系统信息，包括镜像和容器数。。
docker --help   # 帮助

### 镜像命令

docker images  #列出本地主机中的镜像

[root@ncayu8847 ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
golang        latest    46ae95f04a69   3 weeks ago    964MB
postgres      latest    b2261d3c6ce0   3 weeks ago    376MB
redis         latest    2e50d70ba706   3 weeks ago    117MB


# 解释
REPOSITORY  镜像的仓库源
TAG     		镜像的标签
IMAGE ID   	镜像的ID
CREATED   	镜像创建时间
SIZE     		镜像大小

# 可选项
-a：    			列出本地所有镜像
-q：    			只显示镜像id
--digests： 	显示镜像的摘要信息

# 搜索镜像
docker search mysql

# 下载镜像
docker pull mysql

# 删除镜像
docker rmi -f 镜像id             # 删除单个
docker rmi -f 镜像名:tag 镜像名:tag      # 删除多个
docker rmi -f $(docker images -qa)      # 删除全部

```


## 容器命令


```
# 新建容器并启动

# 命令
docker run [OPTIONS] IMAGE [COMMAND][ARG...]
# 常用参数说明
--name="Name"        # 给容器指定一个名字
-d              # 后台方式运行容器，并返回容器的id！
-i              # 以交互模式运行容器，通过和 -t 一起使用
-t              # 给容器重新分配一个终端，通常和 -i 一起使用
-P              # 随机端口映射（大写）
-p              # 指定端口映射（小结），一般可以有四种写法
ip:hostPort:containerPort
ip::containerPort
hostPort:containerPort (常用)
containerPort
  
# 使用centos进行用交互模式启动容器，在容器内执行/bin/bash命令！
[root@ncayu8847 ~]# docker run -it ubuntu /bin/bash
root@ad756561c781:/# ls			# 注意地址，已经进入到容器内部了！
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@ad756561c781:/# 

root@ad756561c781:/# exit		# 使用 exit 退出容器
exit
[root@ncayu8847 ~]#


### 列出所有运行的容器
docker ps -a

# 命令
docker ps [OPTIONS]
# 常用参数说明
-a    # 列出当前所有正在运行的容器 + 历史运行过的容器
-l    # 显示最近创建的容器
-n=?   # 显示最近n个创建的容器
-q    # 静默模式，只显示容器编号。


### 退出容器
exit     # 容器停止退出
ctrl+P+Q   # 容器不停止退出


### 启动停止容器
可以指定容器id或者容器名

docker start 容器名    # 启动容器
docker restart 容器名   # 重启容器
docker stop 容器名     # 停止容器
docker kill 容器名     # 强制停止容器

###删除容器

docker rm 容器id          # 删除指定容器
docker rm -f $(docker ps -a -q)  # 删除所有容器
docker ps -a -q|xargs docker rm  # 删除所有容器


### 进入正在运行的容器
docker exec -it 容器名 /bin/bash

docker attach 容器id
# 区别
# exec  是在容器中打开新的终端，并且可以启动新的进程
# attach 直接进入容器启动命令的终端，不会启动新的进程


### root权限进入容器
docker exec -it  --user root  ubuntu /bin/bash


### 将宿主机的mysqltest.sql文件复制到容器里
docker cp /data/mysqltest.sql mysql:/root/

### 从容器内拷贝文件到主机上
docker cp 容器id:容器内路径 目的主机路径

### 导入镜像文件
docker load -i supermarket.1.6.8.tar


### 查看日志
docker logs -f -t --tail 容器id

# -t 显示时间戳 
# -f 打印最新的日志
# --tail 数字 显示多少条！

### 查看容器中运行的进程信息
docker top 容器id

```


```
### 查看容器的元数据
docker inspect 容器id

# docker inspect ad756561c781

[
    {
        "Id": "ad756561c781f643ddb54656b2e6944172630d513055059461fc925a1743cdbc",
        "Created": "2022-07-20T06:23:21.511515613Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,

```

## 批量删除Docker容器

docker rm mysql  # 删除Docker容器

批量删除已经停止的容器


```
# 方法一

#显示所有的容器，过滤出Exited状态的容器，取出这些容器的ID，

sudo docker ps -a|grep Exited|awk '{print $1}'

#查询所有的容器，过滤出Exited状态的容器，列出容器ID，删除这些容器

sudo docker rm `docker ps -a|grep Exited|awk '{print $1}'`

```


```
# 方法二
#删除所有未运行的容器（已经运行的删除不了，未运行的就一起被删除了）
sudo docker rm $(sudo docker ps -a -q)

# root用户
docker rm $(docker ps -a -q)

```


```
# 方法三
#根据容器的状态，删除Exited状态的容器

sudo docker rm $(sudo docker ps -qf status=exited)

```
Docker 1.13版本以后，可以使用 docker containers prune 命令，删除孤立的容器。


```
#Docker 1.13版本以后，可以使用 docker containers prune 命令，删除孤立的容器。
sudo docker container prune
#删除所有镜像
sudo docker rmi $(docker images -q)

```

## Docker 容器清理空间


```
# 删除没被container 使用的所有image
docker volumn /  image purge 

docker image purge 

删除没被container 使用的所有image,  (之前使用过的会被保留,docker ps -a查看)

```


```
删除所有关闭的容器：

docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs docker rm

删除所有dangling镜像（即无tag的镜像）：

docker rmi $(docker images | grep "^<none>" | awk "{print $3}")

```

dangling是一种特殊的，不会再被使用到的镜像，docker有专门清理dangling镜像的命令


```
# 群友分享的命令
docker image prune -a -f

删除所有dangling数据卷（即无用的Volume）：

docker volume rm $(docker volume ls -qf dangling=true)

```


```
[root@localhost ~]# docker image prune --help

Usage:  docker image prune [OPTIONS]

Remove unused images

Options:
  -a, --all             Remove all unused images, not just dangling ones
      --filter filter   Provide filter values (e.g. 'until=<timestamp>')
  -f, --force           Do not prompt for confirmation 不提示确认

```

## docker其他命令（储备）


```
sudo docker info：显示系统级别的信息，比如容器和镜像的数量等。
 
docker container ls：默认只列出正在运行的容器，-a 选项会列出包括停止的所有容器。
 
docker image ls：列出镜像信息，-a 选项会列出 intermediate 镜像(就是其它镜像依赖的层)。
 
docker volume ls：列出数据卷。
 
docker network ls：列出 network。

```

## docker image 子命令
| 命令 |描述  |
|--|--|
| docker image build |从Docker文件构建映像  |
| docker image history |  显示映像的历史记录|
|docker image import  |  从tarball导入内容以创建文件系统映像|
|docker image inspect  |  显示一个或多个映像的详细信息|
|docker image load | 从tar存档或STDIN加载映像 |
|docker image ls | 列出映像 |
| docker image prune | 删除未使用的映像 |
| docker image pull |从注册表中拉出映像或存储库  |
| docker image push | 将映像或存储库推送到注册表 |
|docker image rm  | 删除一个或多个映像 |
| docker image save | 将一个或多个映像保存到tar存档(默认情况下流式传输到STDOUT) |
| docker image tag | 创建引用SOURCE_IMAGE的标签TARGET_IMAGE |

## docker网卡的IP地址修改
可以在Docker主机上修改/etc/docker/daemon.json文件,添加如下内容:

```
{
  "bip": "192.168.1.5/24",  
}
```
这会将Docker容器分配IP的子网修改为192.168.1.0/24,并且默认网关设置为192.168.1.1。
重启Docker后生效,此后创建的容器会在此子网分配IP。

## vim命令全屏使用

解决docker终端宽度、高度显示不正确


```
docker exec -it --env COLUMNS=`tput cols` --env LINES=`tput lines` your_container_name /bin/bash
```
注：其中tput cols tput lines 为宿主机显示正常的变量值

## docker-compose


```
# 1、查看配置命令

docker-compose config

# 2、后台启动：
docker-compose up -d

# 3、构建镜像：
docker-compose bulid

# 4、下载镜像：
docker-compose pull

# 5、查看运行的镜像：
docker-compose ps

# 6、查看进程：
docker-compose top

# 7、启动已存在的容器命令：
docker-compose start

# 8、停止正在运行的容器命令：
docker-compose stop

# 9、查看服务日志输出：
docker-compose logs

```
