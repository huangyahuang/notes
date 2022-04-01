# docker

基于Linux，系统运行单元—进程，docker将进程进行了隔离，操作系统层面的虚拟化技术。

![image-20220326223051944](/Users/huangya/Library/Application Support/typora-user-images/image-20220326223051944.png)

docker仓库：docker hub

```bash
# docker拉取镜像 
docker pull [image]
# 查看镜像
docker images
# 运行镜像 name 容器名称 -p 宿主端口：容器端口 -d 镜像名称 
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql:latest
# 查看容器
docker ps -a
# 停止容器
docker stop id或容器名称
# 启动容器
docker start id或容器名称
# 删除容器
docker rm -f id或容器名称
# 删除镜像
docker rmi xx
# 进入docker执行命令
docker exec -it some-mysql /bin/sh

```



# 数据卷

## 挂载一个主机目录作为数据卷

1. 在主机的 /root 目录下新建一个文件夹 docker

2. 命令 docker run -it -v 主机目录: 容器内目录

3. 将主机上的 Users/huangya文件夹下面的docker的文件夹与容器内的home文件夹绑定

   ```
   mkdir docker
   docker run -it -v /Users/huangya/docker:/home redis /bin/bash
   ```

## 启动一个挂载数据卷的容器

1. 创建一个数据卷 docker volume create my-vol　

2. 查看所有数据卷 docker volume ls

3. 查看指定的数据卷信息 docker volume inspect my-vol

4. 启动一个挂载数据卷的容器

   ```
   docker run -d -p 6379:6379 --name redisdemo --mount source=my-vol,target=/usr/share/nginx/html redis:latest
   ```

存在问题：在主机上找不到数据卷的目录，是因为docker是基于Linux系统运行的，在macos上创建的时候是模拟Linux系统，卷挂载上在Linux系统里面。



# docker-compose

用配置文件去创建一个容器

```yml
version: "3"  #docker-compsot版本
services:
  redis:
    image: redis:latest #镜像名称和tag
    container_name: redisdemo1 #容器名称
    ports:
      - "63791:6379" # 端口
```

启动命令：-f指定文件 默认文件docker-compose.yal -d 启动守护式容器（在后台启动容器） up 自动完成构建镜像，创建服务，启动服务，关联服务相关容器等一系列操作。

docker-compose -f docker-compose.yml up -d