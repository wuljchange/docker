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
- --name="test"，为容器指定名称
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
