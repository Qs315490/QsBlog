---
title: Docker
date: 2022-05-07 12:44:00
tags: [Docker]
categories: [Docker]
---

Docker 是一个容器化的技术，它可以将应用程序分割成小的容器，并将容器连接起来，从而实现应用程序的分布式运行。

# 文档资料来源
{% link Docker教程::https://jiajially.gitbooks.io/dockerguide/content/index.html %}

## 命令行
命令|说明
-|-
[attach](#attach)|执行容器内的命令
[build](#build)|构建镜像
[create](#create)|创建容器
[commit](#commit)|提交容器内的文件
[cp](#cp)|复制文件到容器
[exec](#exec)|在运行中的容器内执行程序
[images](#images)|查看镜像
[logs](#logs)|查看容器日志
[pull](#pull)|拉取镜像
[push](#push)|推送镜像
[ps](#ps)|查看当前运行的容器
[port](#port)|查看容器端口
[run](#run)|运行容器
[rm](#rm)|删除容器
[rmi](#rmi)|删除镜像
[start](#start)|启动已经停止的容器
[stop](#stop)|停止正在运行容器
[restart](#restart)|重启容器
stats|查看容器状态
[search](#search)|搜索镜像
[kill](#kill)|杀死容器
[tag](#tag)|标记镜像
[import](#import)|导入镜像
[export](#export)|导出镜像
[top](#top)|查看容器运行状态
[-i](#前台运行)|交互式操作
[-t](#前台运行)|以终端模式运行
[-d](#后台运行)|后台运行
[-p](#端口映射)|指定端口
[--name](#容器命名)|指定容器名称
[--net](#容器网络)|删除容器
-e, --env=[]|配置环境变量

### 容器生命周期管理
#### create
创建容器
```sh
docker create <镜像名称>[:镜像版本] [命令] [参数]
```
```sh
docker create ubuntu:latest /bin/echo "Hello World"
```

#### run
创建并运行容器
```sh
docker run <镜像名称>[:<镜像版本>] [命令] [参数]
```
```sh
docker run ubuntu:latest /bin/echo "Hello World"
```

#### exec
在容器内执行程序
```sh
docker exec -it <容器 名称 或 ID> <命令> [参数]
```

#### start
启动容器
```sh
docker start <容器 名称 或 ID>
```

#### stop
停止容器
```sh
docker stop <容器 名称 或 ID>
```

#### restart
重启容器
```sh
docker restart <容器 名称 或 ID>
```

#### kill
杀死容器
```sh
docker kill <容器 名称 或 ID>
```
这会强制停止容器。

#### rm
删除容器
```sh
docker rm <容器 名称 或 ID>
```
这将删除一个已经停止运行的容器，若容器正在运行，则将会使docker报错，停止容器再删除，或者加上-f参数强制删除（不建议）。

#### pause
暂停容器中所有的进程
```sh
docker pause <容器 名称 或 ID>
```

#### unpause
恢复容器中所有的进程
```sh
docker unpause <容器 名称 或 ID>
```

### 容器操作
#### ps
查看正在运行的容器
```sh
docker ps
```
查看所有容器
```sh
docker ps -a
```
查看最新的容器
```sh
docker ps -l
```

#### inspect
查看容器信息
```sh
docker inspect <容器 名称 或 ID>
```

#### top
查看容器正在运行的进程
```sh
docker top <容器 名称 或 ID>
```

#### attach
使用 attach 命令有时候并不方便。当多个窗口同时 attach 到同一个容器的时候，所有窗口都会同步显示。当某个窗口因命令阻塞时,其他窗口也无法执行操作了。
```sh
docker attach <容器 名称 或 ID>
```
```sh
docker attach ubuntu01
```
在命令退出时容器将自动退出,好在attach是可以带上--sig-proxy=false来确保CTRL-D或CTRL-C不会关闭容器。

#### logs
查看容器日志
```sh
docker logs <容器 名称 或 ID>
```
参数|描述
-|-
-f|跟踪日志输出
--since|指定时间之后的日志输出
-t|显示日志的时间戳
--tail|指定日志输出的行数

#### wait
等待容器停止, 然后返回容器退出代码
```sh
docker wait <容器 名称 或 ID>
```

#### export
导出容器的镜像
```sh
docker export <容器 名称 或 ID>
```
导出容器的镜像到文件
```sh
docker export -o <文件名> <容器 名称 或 ID>
```

#### port
查看容器的端口映射
```sh
docker port <容器 名称 或 ID>
```

### 容器rootfs操作
#### commit
提交更改到镜像
```sh
docker commit <容器 名称 或 ID> <镜像名称>[:<镜像版本>]
```
|参数|描述|
|----|-----|
|-a|指定提交的作者|
|-m|指定提交的镜像描述|
|-p|提交时将容器暂停|

提交时附带更改信息
```sh
docker commit -m "提交更改" <容器 名称 或 ID> <镜像名称>[:<镜像版本>]
```

#### cp
复制文件到容器内，或容器文件内到容器外
```sh
docker cp <文件名> <容器 名称 或 ID>:/<目标路径>
```
```sh
docker cp <容器 名称 或 ID>:/<源路径> <文件名>
```

#### diff
查看容器内文件的差异(更改)
```sh
docker diff <容器 名称 或 ID>
```

### 镜像仓库操作
#### login
登录镜像仓库
```sh
docker login <仓库地址>
```
参数|描述
-|-
-u|指定用户名
-p|指定密码

#### logout
退出镜像仓库
```sh
docker logout
```

#### pull
获取镜像
```sh
docker pull <镜像名称>[:<镜像版本>]
```
列如：获取Ubuntu最新镜像
```sh
docker pull ubuntu:latest
```

#### push
推送镜像
```sh
docker push <镜像名称>[:<镜像版本>]
```

#### search
搜索镜像
```sh
docker search <镜像名称>
```
参数|描述
-|-
--automated|只显示自动构建的镜像
--no-trunc|显示完整的镜像描述
-f <过滤条件>|过滤指定的镜像

过滤条件参数|描述
-|-
NAME|镜像仓库源名称
DESCRIPTION|镜像描述
OFFICIAL|是否是官方镜像
stars|镜像的星级
AUTOMATED|是否是自动构建的镜像

### 本地镜像管理
#### images
查看本地存储的镜像
```sh
docker images
```
展示完整镜像ID
```sh
docker images --no-trunc
```
展示没有TAG的镜像
```sh
docker images --filter "dangling=true"
```

#### rmi
删除镜像
```sh
docker rmi <镜像名称>[:<镜像版本>]
```

#### tag
标记镜像
```sh
docker tag <镜像名称>[:<镜像版本>] <标记名称>[:<标记版本>]
```

#### build
构建镜像
参数|描述
-|-
--build-arg=[]|设置镜像创建时的变量
--cpu-shares|设置 cpu 使用权重
--cpu-period|限制 CPU CFS周期
--cpu-quota|限制 CPU CFS配额
--cpuset-cpus|指定使用的CPU id
--cpuset-mems|指定使用的内存 id
--disable-content-trust|忽略校验，默认开启
-f|指定要使用的Dockerfile路径
--force-rm|设置镜像过程中删除中间容器
--isolation|使用容器隔离技术
--label=[]|设置镜像使用的元数据
-m|设置内存最大值
--memory-swap|设置Swap的最大值为内存+swap，"-1"表示不限swap
--no-cache|创建镜像的过程不使用缓存
--pull|尝试去更新镜像的新版本
--quiet, -q|安静模式，成功后只输出镜像 ID
--rm|设置镜像成功后删除中间容器
--shm-size|设置/dev/shm的大小，默认值是64M
--ulimit|Ulimit配置
--squash|将 Dockerfile 中所有的操作压缩为一层
--tag, -t: 镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签
--network: 默认 default。在构建期间设置RUN指令的网络模式

#### history
查看镜像构建历史
参数|描述
-|-
-H|以可读的格式打印镜像大小和日期，默认为true
--no-trunc|显示完整的提交记录
-q|仅列出提交记录ID

#### save
保存镜像
```sh
docker save <镜像名称>[:<镜像版本>] > -o <镜像文件名>
```

#### load
加载 save 保存的镜像
```sh
docker load -i <镜像文件名>
```
参数|描述
-|-
-i <镜像文件名>|镜像文件名, 代替STDIN
--quiet, -q|安静模式

#### import
从归档文件导入镜像
```sh
docker import <镜像文件名>|<网址> <自定义镜像名称>[:<自定义镜像版本>]
```

### 参数
#### 后台运行
```sh
docker run -d <镜像名称>[:<镜像版本>] [命令] [参数]
```
```sh
docker run -d --name=hello ubuntu:latest /bin/echo "Hello World"
```
查看返回信息
```
$ docker logs hello
Hello World
Hello World
···
```
让容器以后台方式运行，并不是加一个 -d 参数就可以，命令行COMMAND所执行的动作必须为持续运行的状态。

#### 前台运行
Docker会启动这个container，同时将当前的命令行窗口挂载到container的标准输入，标准输出和标准错误中。也就是container中所有的输出，你都可以再当前窗口中查看到。甚至docker可以虚拟出一个TTY窗口，来执行信号中断。
```sh
docker run -it <镜像名称>[:<镜像版本>] <命令> [参数]
```
如果在执行run命令时没有指定-a，那么docker默认会挂载所有标准数据流，包括输入输出和错误。你可以特别指定挂载哪个标准流。
```sh
docker run -a stdin -a stdout -it <镜像名称>[:<镜像版本>] [命令] [参数]
```

#### 容器命名
给container 命名有三种方式：
1. 使用UUID长命 ("f78375b1c487e03c9438c729345e54db9d20cfa2ac1fc3494b6eb60872e74778")，在创建容器时返回的id就是这个。
2. 使用UUID短命令("f78375b1c487")，当执行查询时，查到的dockerID就是这个。
3. 使用 "--name=evil_ptolemy" ,若不加此指令，docker会自动给新创建出来的容器分配一个唯一的name

#### 容器退出后清除
Clean up (--rm) 指在容器运行完之后自动清除

#### 容器数据卷
-v|--volume: 挂载一个文件或目录到指定容器中去，实现容器中数据持久化。  
"--volume=<本地目录>:<容器目录>"

发现容器中已经成功挂载数据卷，但是如果你对系统是CentoOS7系统，你会发现，无法访问test，说明权限不够，是因为CentOS7中的安全模块selinux把权限禁掉了，所以要在运行的时候加上特权："--privileged=true"

#### 端口映射
-p|--publish: 端口映射，容器内部的端口可以映射到外部的端口。  
"--publish=<主机端口>[/<协议>]:<容器端口>[/<协议>]"  
"--publish=<主机IP>:<主机端口>:<容器端口>"  
-P|--publish-all: 允许容器内部的所有端口都映射到外部的端口。

#### 容器网络
--net: 指定容器运行的网络。  
"--net=<网络名称>"
类型|描述
-|-
bridge|桥接网络
host|主机网络
none|无网络
container:<容器名称或ID>| 使用已有容器的网络配置

#### 容器主机名
-h|--hostname: 指定容器的主机名。

#### 容器DNS
-dns: 指定容器的DNS服务器。
