# Docker 使用笔记


## 常用命令

几个很常用的命令表

| 命令                                      | 描述               |
| ----------------------------------------- | ------------------ |
| docker images<br />等同于 docker image ls | 查看下载的镜像列表 |
| docker ps -a                              | 已安装的容器列表   |
| docker ps                                 | 当前运行的容器列表 |

### 操作镜像

#### 搜索镜像

```sh
docker search redis
```

![image-20220906143448944](/images/image-20220906143448944.png)

- NAME 镜像的名字
- STARS 就是这个镜像获得的小星星数
- OFFICIAL 是否为官方镜像，为了安全起见，还是下载官方镜像吧。我曾经拉取的 PHP 镜像里有矿机

#### 下载镜像

```sh
docker pull 镜像名称:镜像版本号
```

- latest 版本号代表最新的

演示

```sh
E:\>docker pull redis:latest
latest: Pulling from library/redis
7a6db449b51b: Pull complete
05b1f5f3b2c0: Pull complete
f0036f71a6fe: Pull complete
cd7ddcecb993: Pull complete
8cfc9a467ed7: Pull complete
2a9998409df9: Pull complete
Digest: sha256:495732ba570db6a3626370a1fb949e98273a13d41eb3e26f7ecb1f6e31ad4041
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
```

#### 删除镜像

```sh
docker rmi 镜像名称
```

### 操作容器

| 命令                  | 描述     |
| --------------------- | -------- |
| docker start 容器id   | 启动容器 |
| docker stop 容器id    | 停止容器 |
| docker restart 容器id | 重启容器 |

#### 利用镜像创建容器

其实安装镜像，就是创建容器。

`docker run 各种参数 镜像文件`命令的参数比较丰富。

| 参数                   | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| -d<br />等同 --detach  | Run container in background and print container ID<br />就是让容器以后启动了在后台运行而不是前台 |
| --name 容器名称        | 给容器起个名字，最好带上版本号，因为可能安装多个版本镜像     |
| -e "name=tony"         | 设置环境变量                                                 |
| -p 宿主机端口:容器端口 | 端口映射                                                     |

演示

```sh
docker run -d --name redis -p 6379:6378 redis:latest
```

#### 删除容器

```sh
docker rm 容器id
```

#### 进入容器

```sh
docker exec -it 容器名称 /bin/bash
```

## 构建自己的镜像


