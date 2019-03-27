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
- 

