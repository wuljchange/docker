## docker常用命令

### 容器生命周期管理
- run
- start/stop/restart
- kill
- rm
- pause/unpause
- create
- exec

#### docker run 命令详解
##### 语法
    docker run [OPTIONS] image [COMMAND] [ARG]
    
常用的options

- -d 后台运行，并返回容器ID
- -i 交互模式运行容器，通常与-t一起使用
- -t 新建一个伪终端
- --name="test"，为镜像生成的容器指定名称
- -e username="test" 设置环境变量
- -m 设置容器使用内存最大值
- --net="bridge" 指定容器的网络连接类型
- --link=[] 添加链接到另一个容器
- --expose=[] 开放一个端口或一组端口

##### 实例
命名实例，后台启动容器  

    docker run --name newname -d image

映射端口，后台启动实例

    docker run -p 80:80 -v /data:/data -d image
    
以交互模式启动一个容器

    docker run -it image /bin/bash
    
    
#### docker start/stop/restart 命令详解
##### 语法
    docker start/stop/restart container
    
##### 实例
    docker start mycontainer
    docker stop mycontainer
    docker restart mycontainer


#### docker kill 命令详解
##### 语法
    docker kill [POTIONS] container
    
常用的options

- -s 向容器发送一个信号

##### 实例
杀掉容器

    docker kill -s KILL mycontainer
 

#### docker rm 命令详解
##### 语法
    docker rm [options] container
    
常用的options

- -f 强制删除容器
- -l 移除容器间的连接
- -v 删除与容器有关的卷

##### 实例
强制删除容器con1,con2

    docker rm -f con1,con2
    
删除link

    docker rm -l link
    
删除容器并删除容器挂载的数据卷

    docker rm -v con
    
    
#### docker pause/unpause 命令详解
##### 实例
暂停或者恢复容器服务

    docker pause/unpause container
    
   
#### docker create 命令详解
##### 实例
使用nginx:latest镜像创建容器并重新命名

    docker create --name mycontainer nginx:latest
    

#### docker exec 命令详解
##### 语法
    docker exec [options] container command
    
##### 实例
在容器中以交互模式运行，并执行脚本

    docker exec -it mynginx /bin/bash test.sh
    
    

### 容器操作
- ps
- inspect
- top
- attach
- events
- logs
- wait
- export
- port

#### docker ps 命令详解
##### 语法
    docker ps [options]
    
常用 options

- -a 显示所有容器，包括未运行的
- -f 根据条件过滤显示
- --format 指定返回值的模版文件
- -l 显示最近创建过的容器
- -n 显示最近创建的n个容器
- -q 静默模式 只显示容器编号
- -s 显示总的文件大小

##### 实例
显示正在运行的容器

    docker ps 
    
显示所有容器，包括未运行

    docker ps -a
    
显示最近创建的5个容器

    docker ps -n 5
    
显示所有创建的容器id

    docker ps -a -q
    

#### docker inspect 命令详解
##### 语法
    docker inspect [options] container/image [arg]
    
常用的 options

- -f 指定返回值的模版文件
- --type 指定返回类型

#### 实例
查看容器的ip

    docker inspect -f '{{.NetworkSettings.IPAddress}}' mycontainer
    

#### docker top 命令详解
##### 语法
    docker top [options] container [ps arg]
    
##### 实例
查看容器的进程信息

    docker top container
    
查看所有运行容器的进程信息

    for container in `docker ps|grep pattern|awk '{print $1}'`;do echo &&docker top $container;done
    

#### docker events 命令详解
##### 语法
    docker events [options]
    
options

- -f 条件过滤
- --since 显示创建时间大于改时间的所有事件
- --until 流水时间显示到指定的时间为止

#### 实例
    docker events -f "image"="name" --since="timestamp"
    
#### docker logs 命令详解
跟踪容器的日志输出

    docker logs -f
    
显示大于某个时间的最近10条日志

    docker logs --since="timestamp"  --tail=10 container
    
    
#### docker port 命令详解
列出指定容器的端口号映射

    docker port container
    
    
### 容器rootfs命令
- commit
- cp
- diff


#### docker commit
##### 语法
从容器创建一个新的镜像

    docker commit [options] container [resp:tag]
    
options

- -a 指定作者
- -c 用Dockerfile来创建镜像
- -m 添加说明信息
- -p 在提交镜像的同时将镜像暂停


##### 实例
    docker commit -a "name" -m "test" -p container wuljchange:tag
    
    
#### docker cp命令
将主机的目录文件拷贝到容器的某个目录

    docker cp /test/ container:/tmp
    
将容器的目录拷贝到主机的目录

    docker cp container:/tmp /test/


#### docker diff
检查容器文件结构的更改

    docker diff container
    
    
#### docker login/logout
登录dockerhub
    
    docker login -u name -p password
    
退出dockerhub

    docker logout
    

#### docker pull/push
拉取所有tag镜像
    
    docker pull -a nametag
    
    
#### docker search 
列出收藏数不小于10的镜像

    docker search -s 10 tag
    

### 本地镜像管理 **
- images
- rmi
- tag
- build
- history
- save
- import


#### docker images
列出本地所有的images，包括中间层镜像，默认过滤掉中间层镜像

    docker images -a
    
只显示镜像id

    docker images -a -q
    
       
#### docker rmi
删除本地所有镜像

    docker rmi $(docker images -a -q) 
    
删除某个镜像 or 删除镜像名为某个值的所有的镜像

    docker rmi image-id
    docker rmi $(docker images image-name -q)
    
强制删除镜像

    docker rmi -f w3cschool/ubuntu:v4
    
不移除该镜像的过程镜像

    docker rmi --no-prune image-id
    

#### docker tag
标记本地镜像，用于上传仓库

    docker tag local-image respority-image
    

#### docker build
Dockerfile构建本地镜像

    docker build -t image-name .
    
Dockerfile不在当前目录，指定要使用的dockerfile文件

    docker build -t image-name -f /root/dockerapp/
    
一些常用创建参数 cpu使用权重,内存最大值,不使用缓存,成功后只输出镜像id,创建成功后删除中间容器

    docker build -t image-name . --cpu-shares='0.1' -m=50M --no-cache -q  --rm
    

#### docker history
查看本地镜像创建历史记录
    
    docker history --no-trunc image-name
    
仅显示提交id

    docker history image -q
    
    
#### docker save
将镜像w3cschool/ubuntu:v3 生成my_ubuntu_v3.tar文档

    docker save -o my_ubuntu_v3.tar w3cschool/ubuntu:v3
    
    
#### docker import
从镜像归档文件my_ubuntu_v3.tar创建镜像，命名为w3cschool/ubuntu:v4
    
    docker import my_ubuntu_v3.tar w3cschool/ubuntu:v4