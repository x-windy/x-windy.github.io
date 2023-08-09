---
title: "Docker"
description: ""
author: "XW"
draft: false
---

# 容器(Container)

> 应用程序在运行时相互独立互不干扰
>
> 只隔离应用程序的运行时环境，容器之间可以共享同一个操作系统
>
> 类似码头(主机)和集装箱(容器)

## Docker

> Docker 是一个开源的应用容器引擎，基于 [Go 语言](https://www.runoob.com/go/go-tutorial.html) 并遵从 Apache2.0 协议开源。
>
> Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。
>
> 容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。
>
> [Docker Docs: How to build, share, and run applications | Docker Documentation](https://docs.docker.com/)

![docker](https://docs.docker.com/assets/images/architecture.svg)




### 名词概念

`dockerfile(源码/配置文件)`

`image(镜像/可执行程序/类)`

`container(容器/进程/对象)`

`registry(容器仓库)` [Docker Hub]([https://hub.docker.com](https://hub.docker.com/)) 

`docker client(负责处理用户输入的指令/client)`

`docker demon(服务器)`


| 概念                   | 说明                                                         |
| :--------------------- | :----------------------------------------------------------- |
| Docker 镜像(Images)    | Docker 镜像是用于创建 Docker 容器的模板，比如 Ubuntu 系统。  |
| Docker 容器(Container) | 容器是独立运行的一个或一组应用，是镜像运行时的实体。         |
| Docker 客户端(Client)  | Docker 客户端通过命令行或者其他工具使用 Docker SDK (https://docs.docker.com/develop/sdk/) 与 Docker 的守护进程通信。 |
| Docker 主机(Host)      | 一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。       |
| Docker Registry        | Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。Docker Hub([https://hub.docker.com](https://hub.docker.com/)) 提供了庞大的镜像集合供使用。一个 Docker Registry 中可以包含多个仓库（Repository）；每个仓库可以包含多个标签（Tag）；每个标签对应一个镜像。通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本。我们可以通过 **<仓库名>:<标签>** 的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 **latest** 作为默认标签。 |
| Docker Machine         | Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。 |

### 常用指令

| docker client         | docker demon      | Registry |
| --------------------- | ----------------- | -------- |
| docker build(编译)    | dockerfile->image |          |
| docker run(运行)      | image->container  |          |
| docker pull(下载镜像) |                   | image    |

```shell
# 启动服务
systemctl start docker
# exit或CTRL+D退出容器

# 查看出本地主机上的所有镜像
docker images
```

> 同一仓库源可以有多个 TAG，代表这个仓库源的不同个版本，如 ubuntu 仓库源里，有 15.10、14.04 等多个不同的版本，我们使用 `REPOSITORY:TAG` 来定义**不同的镜像**。

各个选项说明:

- **`REPOSITORY`：**表示镜像的仓库源
- **`TAG`：**镜像的标签
- **`IMAGE ID`：**镜像ID
- **`CREATED`：**镜像创建时间
- **`SIZE`：**镜像大小

#### 运行交互式的容器

```shell
docker run -i -t [指定运行的镜像] [启动容器执行的命令]
docker run -it [指定运行的镜像] [启动容器执行的命令]
```

参数：

- `-t`: 在新容器内指定一个伪终端或终端。
- `-i`: 允许你对容器内的标准输入 (STDIN) 进行交互。

#### 进程方式运行的容器

```shell
docker run -d [指定运行的镜像] [启动容器执行的命令]
# 查看容器运行的进程
docker ps
docker ps -a
```

输出详情介绍：

- **`CONTAINER ID`:** 容器 ID。

- **`IMAGE`:** 使用的镜像。

- **`COMMAND`:** 启动容器时运行的命令。

- **`CREATED:`** 容器的创建时间。

- **`STATUS`:** 容器状态。
  - 状态有7种：
    - `created`（已创建）
    - `restarting`（重启中）
    - `running` 或 `Up`（运行中）
    - `removing`（迁移中）
    - `paused`（暂停）
    - `exited`（停止）
    - `dead`（死亡）
- **`PORTS`:** 容器的端口信息和使用的连接类型（tcp\udp）。
- **`NAMES`:** 自动分配的容器名称。

#### 查看容器内的标准输出

```shell
docker logs [CONTAINER ID]/[NAMES]
```

### 镜像

#### 查找镜像

>  https://hub.docker.com/

```
docker search [镜像]
```

- **`NAME`:** 镜像仓库源的名称
- **`DESCRIPTION`:** 镜像的描述
- **`OFFICIAL`:** 是否 docker 官方发布
- **`stars`:** 类似 Github 里面的 star，表示点赞、喜欢的意思。
- **`AUTOMATED`:** 自动构建。

#### 获取镜像

```
docker pull [镜像]
```

#### 创建镜像

##### 更新镜像

> 先创建一个容器`docker run -it`

```shell
# 在容器内
apt-get update
exit
```

##### 修改容器

```shell
docker commit -m="has update" -a="runoob" [CONTAINER ID] [NAMES]
docker commit -m="has update" -a="runoob" e218edb10161 runoob/ubuntu:v2
# 查看新的镜像
docker images
```

参数说明：

- **`-m`:** 提交的描述信息
- **`-a`:** 指定镜像作者
- **e218edb10161：**容器 ID
- **runoob/ubuntu:v2:** 指定要创建的目标镜像名

##### 构建镜像

```dockerfile
# 创建文件
cat Dockerfile

FROM    centos:6.7
MAINTAINER      Fisher "fisher@sudops.com"

RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd runoob
RUN     /bin/echo 'runoob:123456' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
EXPOSE  22
EXPOSE  80
CMD     /usr/sbin/sshd -D


# 编译创建镜像
docker build -t runoob/centos:6.7 .
```

> 每一个指令的前缀都必须是大写的。
>
> `RUN` 指令告诉docker 在镜像内执行命令，安装了什么

参数说明：

- **`-t`** ：指定要创建的目标镜像名
- **`.`** ：Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径

##### 设置镜像标签

```shell
docker tag [IMAGE ID] runoob/centos:dev
```

> `runoob/centos:dev`
>
> 用户名称`/`镜像源名(repository name)`:`新的标签名(tag)

#### 删除镜像

```
docker rmi [IMAGE ID]
```

### 容器

#### 启动容器

```shell
# 交互模式
docker run -it [指定运行的镜像] [启动容器执行的命令]
# 进程模式
docker run -d [指定运行的镜像] [启动容器执行的命令]

docker run -itd --name ubuntu-test ubuntu /bin/bash


# 不指定一个镜像的版本标签,默认使用ubuntu:latest镜像
# 启动已停止的容器
docker start [CONTAINER ID]
```

#### 进入容器

```shell
# 如果从这个容器退出，会导致容器的停止
docker attach
# 会退出容器终端，但不会导致容器的停止
docker exec
```

#### 停止容器

```shell
docker stop [CONTAINER ID]
# 重启容器
docker restart [CONTAINER ID]
```

#### 导出容器和导入容器

##### 导出容器

> 导出容器快照

```shell
docker export [CONTAINER ID] > [容器快照名]
docker export 1e560fca3906 > ubuntu.tar
```

##### 导入容器

```shell
docker import [容器快照名]
docker import http://example.com/exampleimage.tgz example/imagerepo
```

#### 删除容器

```shell
docker rm -f [CONTAINER ID]
# 清理掉所有处于终止状态的容器
docker container prune
```

#### 运行web项目

```shell
docker pull training/webapp  # 载入镜像
docker run -d -P training/webapp python app.py
# -p设置端口
docker run -d -p 5000:5000 training/webapp python app.py
```

参数说明:

- **`-d`:**让容器在后台运行。
- **`-P`:**将容器内部使用的网络端口随机映射到我们使用的主机上。

##### 查看端口

>  查看指定 （ID 或者名字）容器的某个确定端口映射到宿主机的端口号。

```shell
docker port [CONTAINER ID]/[NAMES]
```

##### 查看 WEB 应用程序日志

```shell
docker logs -f [CONTAINER ID]/[NAMES]
```

**`-f`:** 让 **docker logs** 像使用 **tail -f** 一样来输出容器内部的标准输出。

##### 查看WEB应用程序容器的进程

```shell
docker top [CONTAINER ID]/[NAMES]
```

##### 检查 WEB 应用程序

> **`docker inspect`**：查看 Docker 的底层信息。它会返回一个 JSON 文件记录着 Docker 容器的配置和状态信息。

```shell
docker inspect [CONTAINER ID]/[NAMES]
```

##### 移除WEB应用容器

> 删除容器时，容器必须是停止状态

```
docker rm [CONTAINER ID]/[NAMES]
```



#### 查询最后一次创建的容器

```shell
docker ps -l
```

#### 连接Docker容器

> 指定容器绑定的网络地址，比如绑定 127.0.0.1

```shell
# 可以通过访问 127.0.0.1:5001 来访问容器的 5000 端口
docker run -d -p 127.0.0.1:5001:5000 training/webapp python app.py
# 绑定 UDP 端口(默认TCP)
docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py

# 查看端口的绑定情况
docker port adoring_stonebraker 5000
```

使用 **-p** 标识来指定容器端口绑定到主机端口。

两种方式的区别是:

- **`-P` :**是容器内部端口**随机**映射到主机的端口。
- **`-p` :** 是容器内部端口绑定到**指定**的主机端口。

#### 互联Docker 容器

> 端口映射并不是唯一把 docker 连接到另一个容器的方法。
>
> docker 有一个连接系统允许将多个容器连接在一起，共享连接信息。
>
> docker 连接会创建一个父子关系，其中父容器可以看到子容器的信息。

##### 容器命名

> 当我们创建一个容器的时候，docker 会自动对它进行命名。

```shell
# 当我们创建一个容器的时候，docker 会自动对它进行命名
docker run -d -P --name [指定名称] 
```

##### 新建网络

```shell
# 创建一个新的 Docker 网络
docker network create -d bridge test-net
# 查看docker网络
docker network ls
```

参数说明：

**`-d`**：参数指定 Docker 网络类型，有 bridge、overlay。

​			其中 overlay 网络类型用于 Swarm mode

##### 连接容器

> 运行一个容器并连接到新建的 test-net 网络

```shell
docker run -itd --name test1 --network test-net ubuntu /bin/bash
```

> 打开新的终端，再运行一个容器并加入到 test-net 网络

```shell
docker run -itd --name test2 --network test-net ubuntu /bin/bash
```

> 安装ping
>
> 可以在一个容器里安装好，提交容器到镜像，在以新的镜像重新运行以上俩个容器
>
> ```shell
> docker commit -m="说明" -a="作者" [CONTAINER ID] [NAMES]
> ```

```shell
apt-get update
apt install iputils-ping
```

##### 配置DNS

> 在主机上 `/etc/docker/daemon.json` 设置全部容器的 DNS

```json
{
  "dns" : [
    "114.114.114.114",
    "8.8.8.8"
  ]
}
```

> 设置后，启动容器的 DNS 会自动配置为 114.114.114.114 和 8.8.8.8。
>
> 配置完，需要重启 docker 才能生效。
>
> 查看容器的 DNS 是否生效可以使用以下命令，它会输出容器的 DNS 信息：

```shell
docker run -it --rm  ubuntu  cat etc/resolv.conf
```

##### 手动指定容器的配置

> 如果只想在指定的容器设置 DNS
>
> 如果在容器启动时没有指定 **--dns** 和 **--dns-search**，Docker 会默认用宿主主机上的 /etc/resolv.conf 来配置容器的 DNS。

```shell
docker run -it --rm -h host_ubuntu  --dns=114.114.114.114 --dns-search=test.com ubuntu
```

参数说明：

- **`--rm`**：容器退出时自动清理容器内部的文件系统。
- **`-h HOSTNAME` 或者 `--hostname=HOSTNAME`**： 设定容器的主机名，它会被写到容器内的 /etc/hostname 和 /etc/hosts。
- **`--dns=IP_ADDRESS`**： 添加 DNS 服务器到容器的 `/etc/resolv.conf` 中，让容器用这个服务器来解析所有不在 `/etc/hosts` 中的主机名。
- **`--dns-search=DOMAIN`**： 设定容器的搜索域，当设定搜索域为 .example.com 时，在搜索一个名为 host 的主机时，DNS 不仅搜索 host，还会搜索 host.example.com

#### Docker 仓库管理

> 仓库（Repository）是集中存放镜像的地方
>
> [Docker Hub](https://hub.docker.com/)

##### 注册

> 在 [https://hub.docker.com](https://hub.docker.com/) 免费注册一个 Docker 账号。

##### 登录

> 登录需要输入用户名和密码，登录成功后，我们就可以从 `docker hub` 上拉取**自己账号下的全部镜像**。

```shell
docker login
```

##### **退出**

> 退出 docker hub 可以使用以下命令：

```shell
docker logout
```

##### 拉取镜像

你可以通过 `docker search` 命令来查找**官方仓库中的镜像**，并利用 docker pull 命令来将它下载到本地。

以 ubuntu 为关键词进行搜索：

```
docker search ubuntu
docker pull ubuntu 
```

##### 推送镜像

> 用户登录后，可以通过 docker push 命令将自己的镜像推送到 Docker Hub。
>
> 以下命令中的 username 请替换为你的 Docker 账号用户名。

```shell
docker tag ubuntu:18.04 username/ubuntu:18.04
docker image ls
```



### Docker Dockerfile

> Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。

#### 定制nginx 镜像

> **构建好的镜像内会有一个 `/usr/share/nginx/html/index.html` 文件**
>
> 空目录下，新建一个名为 Dockerfile 文件

```shell
mkdir Dockerfile
cd Dockerfile/
vi Dockerfile
# 打印文件文本
cat Dockerfile
```

```dockerfile
FROM nginx
RUN echo '这是一个本地构建的nginx镜像' > /usr/share/nginx/html/index.html
```

- **`FROM`**：定制的镜像都是基于 FROM 的镜像，这里的 nginx 就是定制需要的基础镜像。后续的操作都是基于 nginx。

- **`RUN`**：用于执行后面跟着的命令行命令。有以下俩种格式：

`shell` 格式：

```dockerfile
RUN <命令行命令>
# <命令行命令> 等同于，在终端操作的 shell 命令。
```

`exec` 格式：

```dockerfile
RUN ["可执行文件", "参数1", "参数2"]
# 例如：
# RUN ["./test.php", "dev", "offline"] 等价于 RUN ./test.php dev offline
```

> **注意**：Dockerfile 的指令每执行一次都会在 docker 上新建一层。所以过多无意义的层，会造成镜像膨胀过大。例如：



```dockerfile
FROM centos
RUN yum -y install wget
RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz"
RUN tar -xvf redis.tar.gz
```

以上执行会创建 3 层镜像。可简化为以下格式：

```dockerfile
FROM centos
RUN yum -y install wget \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
    && tar -xvf redis.tar.gz
```

如上，以 **&&** 符号连接命令，这样执行后，**只会创建 1 层镜像**。

#### 开始构建镜像

> 在 Dockerfile 文件的存放目录下，执行构建动作。
>
> 以下示例，通过目录下的 Dockerfile 构建一个 `nginx:v3`（镜像名称:镜像标签）。
>
> **注**：最后的 `.` 代表本次执行的上下文路径

```shell
docker build -t nginx:v3 .
docker images
```

##### 上下文路径

> 上下文路径，是指 docker 在构建镜像，有时候想要使用到本机的文件（比如复制），`docker build` 命令得知这个路径后，**会将路径下的所有内容打包**。
>
> **解析**：由于 docker 的运行模式是 C/S。我们本机是 C，docker 引擎是 S。实际的构建过程是在 docker 引擎下完成的，所以这个时候**无法用到我们本机的文件**。这就需要把我们**本机的指定目录下的文件**一起打包提供给 docker 引擎使用。
>
> **如果未说明最后一个参数**，那么默认上下文路径就是 **Dockerfile 所在的位置**。
>
> **注意**：上下文路径下不要放无用的文件，因为会一起打包发送给 docker 引擎，如果文件过多会造成过程缓慢。

#### 指令详解

> [Docker Dockerfile | 菜鸟教程 (runoob.com)](https://www.runoob.com/docker/docker-dockerfile.html)

| Dockerfile 指令 | 说明                                                         |
| :-------------- | :----------------------------------------------------------- |
| FROM            | 指定**基础镜像**，用于后续的指令构建。                       |
| MAINTAINER      | 指定Dockerfile的作者/维护者。（已弃用，推荐使用`LABEL`指令） |
| LABEL           | 添加镜像的元数据，使用键值对的形式。                         |
| RUN             | 在构建过程中在镜像中执行命令。                               |
| CMD             | 指定容器创建时的默认命令。（可以被覆盖）                     |
| ENTRYPOINT      | 设置容器创建时的主要命令。（不可被覆盖）                     |
| EXPOSE          | 声明容器运行时监听的特定网络端口。                           |
| ENV             | 在容器内部设置环境变量。                                     |
| ADD             | 将文件、目录或远程URL复制到镜像中。                          |
| COPY            | 将文件或目录复制到镜像中。                                   |
| VOLUME          | 为容器创建挂载点或声明卷。                                   |
| WORKDIR         | 设置后续指令的工作目录。                                     |
| USER            | 指定后续指令的用户上下文。                                   |
| ARG             | 定义在构建过程中传递给构建器的变量，可使用 "docker build" 命令设置。 |
| ONBUILD         | 当该镜像被用作另一个构建过程的基础时，添加触发器。           |
| STOPSIGNAL      | 设置发送给容器以退出的系统调用信号。                         |
| HEALTHCHECK     | 定义周期性检查容器健康状态的命令。                           |
| SHELL           | 覆盖Docker中默认的shell，用于RUN、CMD和ENTRYPOINT指令。      |

###  Docker 客户端的所有命令选项

```shell
docker
# 指定命令的具体用法
docker [command] --help
```

