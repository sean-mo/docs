# Docker

## 简介

### Docker Toolbox

> [ToolBox](https://www.docker.com/products/docker-toolbox): Windows/OSX 下 Docker 环境及工具的安装包，提供以下工具的安装：

- Docker Client
- Docker Machine
- Docker Compose (Mac only)
- Docker Kitematic
- VirtualBox

### Kitematic
> [Kitematic](https://docs.docker.com/kitematic/userguide/) 是 docker GUI工具，可视化创建、启动、删除、数据卷等 

### Machine
> [Machine](https://docs.docker.com/machine/) 解决的是操作系统异构安装Docker困难的问题，没有Machine的时候，CentOS是一种，Ubuntu又是一种，AWS又是一种。有了Machine，所有的系统都是一样的安装方式。

### Compose
> [Compose](https://docs.docker.com/compose/) 解决多容器部署的问题，替代Fig，不支持Windows安装。

### Swarm
> [Swarm](https://docs.docker.com/swarm/) 解决Docker集群和调度问题

## 镜像

### 获取镜像

CMD： `docker pull name[:tag]`

示例：

```
// 下载最新版本的镜像，不指定则从registery.hub.docker.com下载
docker pull ubuntu   
// 特定版本                      
docker pull ubuntu:14.04     
// 特定仓库              
docker pull dl.dockerpool.com:500/ubuntu   
```

### 查看镜像

CMD: `docker images`

### 给镜像打标签

CMD: `docker tag source targetName[:Tag]`

示例：

```
docker tag dl.dockerpool.com:5000/ubuntu:latest ubuntu:latest
```
### 获取镜像元数据

CMD: `docker inspect [ImageID]`

示例：https://hacpai.com/article/1427784659823

```
// 获取某镜像ID的元数据
docker inspect 550degag 
// 获取IP地址
docker inspect -f {{.IPAddress}}  
// -f是模板
docker inspect -f '{{if ne 0.0 .State.ExitCode }}{{.Name}} {{.State.ExitCode}}{{ end }}' $(docker ps -aq)
// 返回结果：
// /tender_colden 1
// /clever_mcclintock 126

/grave_bartik 1
```

### 搜寻镜像

CMD: `docker search [team]`

参数：

- --automated=false 仅显示自动创建的IMAGE
- --no-trunc=false 输出不截断显示
- -s, --stars=0 仅显示星级评价以上的IMAGE

示例：

```
docker search mysql
```

### 删除镜像

> 只删除该镜像最新的标签，若只剩下一个标签，则删除整个标签

CMD: `docker rmi [镜像名/镜像ID]`

参数：

- -f 强制删除镜像（不推荐）

示例：

```
docker rmi dl.dockerpool.com:5000/ubuntu
docker rm $(docker ps -q -a) 一次性删除所有的容器
docker rmi $(docker images -q) 一次性删除所有的镜像。
```

### 查看镜像所有容器

`docker ps -a`

### 创建镜像

> 镜像创建的三种方法

- docker commit
- docker build
- DockerFile

#### Docker Commit
> 基于原有的镜像创建新的容器。当容器运行时，发生改变，可以exit容器，提交镜像

```
// aoct/apache2: 创建的镜像名 / 614122c0aabb: 基于的容器ID
docker commit 614122c0aabb aoct/apache2
// -m 为注释， --author为作者
docker commit -m='A new image' --author='Aomine' 614122c0aabb aoct/apache2
```

### 提交镜像

> 设置push的仓库和位置

`docker push user/test:latest`

### 镜像导入与导出

```
// 镜像导入
docker load < node.tar
// 镜像导出
docker save > node.tar
```

## 容器


参数：

- -t 启动一个bash终端
- -i 让容器标准输入保持OPEN状态
- -d 守护状态运行（即在后台运行）
- -p 5000:5000 设置宿主端口：容器端口的映射
- -P 指定一个容器端口（随机分配） 
- -v source:dockerTarget 设置数据映射到本地
- --name web 指定容器的名称为web

```
// OSX 容器路径映射
docker run -v /Users/<path>:/<container path> ...
// WINDOW 容器路径映射 c:\Users
docker run -v /c/Users/<path>:/<container path>
```

```
 // 创建一个停止状态的容器
 docker create -it ubuntu:lastest 
 // 创建一个新容并启动一个容器并运行代码
 docker run -it ubuntu /bin/echo 'hello world'
// 获取容器ce5输出信息
docker logs ce5
// 终止容器ce5
docker stop ce5
// 终止容器10秒后发信息到容器
docker stop ce5 --time=10
// 查看终止的容器
docker ps -a -q
// 启动容器
docker start ce5
// 重启容器
docker restart ce5
//  进入容器，会阻塞所有窗体的进程
docker attach ce5
//  进入容器，并启动一个命令
docker exec -ti 243c32535da7 /bin/bash
// 删除终止容器
docker rm ce5
// 删除容器
// -v, --volumes=false 删除挂载的数据卷
// -f, --force=false 强行删除运行中的容器
// -l, --link=false 删除容器连接，但保留容器
docker kill ce5
// 导出容器
docker export ce5 > ce5.tar
// 导入容器 会丢失所有历史和元数据
cat ubuntu.tar | sudo docker import - test/ubuntu:v1.0
docker import http://example.com/exampleimage.tgz example/imagerepo
// 登录仓库
docker login
// 创建私有仓库
docker run -d -p 5000:5000 registry
docker run -d -p 5000:5000 -v /opt/data/registry:/tmp/registry registry
```

### 数据容量

深入理解docker volume: 

- http://dockone.io/article/128
- http://dockone.io/article/129

假如挂载的时候，不指定宿主机的地址，则自动挂载到/var/lib/docker/vfs/dir，以下是容器/data挂到本地

##### 如果挂载的主机卷非本地卷，即sys(mac)，则有不会成功挂载到主机上 

`docker run -it -v /data ce5`

```
// 创建一个数据卷容器，并设置数据内容存储在容器的/dbData里
docker run -it -v /dbData --name dbdata ubuntu
// --volumes-from 挂载dbdata，启动容器时，会找到dbdata这个目录
docker run -it --volumes-from dbdata --name db1 ubuntu
docker run -it --volumes-from dbdata --name db2 ubuntu
// 也可以从db1位置挂载到db3
docker run -d --name db3 --volumes-from db1 ce5
docker rm -v dbdata 删除指定数据卷。
```

### 端口

ip:hostPort:containerPort
ip::containerPort
hostPort:containerPort

```
// 查看ce5 5000的映射端口
docker port ce5 5000

docker run -it -p 127.0.0.0:5000:5000
docker run -it -p 127.0.0.0::5000
docker run -it -p 5000:5000
```

容器间通信

--name web: 自定义名称

```
// 容器终止后自动删除容器
docker run --rm -ti ce5 
```


## Docker node_modules 保存到镜像中

- NODE_PATH
- Ln -s 软链接
- `node_modules` 安装到code的父级，如
 app/node_modules
 app/src/code
- add . /src  -> npm i -> docker -v . src 

现成示例：

- https://github.com/b00giZm/docker-compose-nodejs-examples

- NPM & Docker: Sharing volumes: http://www.saulshanabrook.com/npm-docker-sharing-volumes/

- [rapid local development vagrant docker node](http://kevzettler.com/programming/2015/06/07/rapid_local_development_vagrant_docker_node.html)

- http://codeflippa.com/questions/30043872/docker-compose-node-modules-not-present-in-a-volume-after-npm-install-succeeds

- http://stackoverflow.com/questions/30925521/node-js-docker-compose-node-modules-disappears


## Docker Machine

### 使用的先决条件（二选一） 

- 安装[virtualBox](https://www.virtualbox.org/wiki/Downloads)
- 通过安装[toolBox](https://www.docker.com/products/docker-toolbox)，将自动安装virtualBox.

```
// 查看HOST
docker-machine ls
// 创建一个default的host
docker-machine create --driver virtualbox default
// 查看ec-crm环境配置
docker-machine env ec-crm
// 连接到ec-crm的shell
eval "$(docker-machine env ec-crm)"
// 停止
docker-machine stop ec-crm
// 启动
docker-machine start ec-crm
// machine间文件传输
docker-machine scp [machine:][path] [machine:][path]
// 更多命令
   - `docker-machine config`
    - `docker-machine env`
    - `docker-machine inspect`
    - `docker-machine ip`
    - `docker-machine kill`
    - `docker-machine provision`
    - `docker-machine regenerate-certs`
    - `docker-machine restart`
    - `docker-machine ssh`
    - `docker-machine rm`
    - `docker-machine start`
    - `docker-machine status`
    - `docker-machine stop`
    - `docker-machine upgrade`
    - `docker-machine url`
```

### Docker Compose

- http://dockone.io/article/332
- http://debugo.com/docker-compose/

安装：

- `curl -L https://github.com/docker/compose/releases/download/1.6.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose`
- $ `chmod +x /usr/local/bin/docker-compose`

```

build 构建或重建服务
help 命令帮助
kill 杀掉容器
logs 显示容器的输出内容
port 打印绑定的开放端口
ps 显示容器
pull 拉取服务镜像
restart 重启服务
rm 删除停止的容器
run 运行一个一次性命令
scale 设置服务的容器数目
start 开启服务
stop 停止服务
up 创建并启动容器

Usage:
  docker-compose [-f=<arg>...] [options] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE           Specify an alternate compose file (default: docker-compose.yml)
  -p, --project-name NAME   Specify an alternate project name (default: directory name)
  --verbose                 Show more output
  -v, --version             Print version and exit

Commands:
  build              Build or rebuild services
  config             Validate and view the compose file
  create             Create services
  down               Stop and remove containers, networks, images, and volumes
  events             Receive real time events from containers
  help               Get help on a command
  kill               Kill containers
  logs               View output from containers
  pause              Pause services
  port               Print the public port for a port binding
  ps                 List containers
  pull               Pulls service images
  restart            Restart services
  rm                 Remove stopped containers
  run                Run a one-off command
  scale              Set number of containers for a service
  start              Start services
  stop               Stop services
  unpause            Unpause services
  up                 Create and start containers
  version            Show the Docker-Compose version information

```
