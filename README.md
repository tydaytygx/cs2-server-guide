# 教程
本教程使用docker compose，旨在提供便利的方式供CS2中文社区玩家开服（Linux平台），并安装插件

metamod

CounterStrikeSharp



**教程将引用以下仓库的镜像，如果需要进一步了解镜像架构请移步下方**

[源仓库https://github.com/joedwards32/CS2](https://github.com/joedwards32/CS2)


# 准备工作
## 租赁一台虚拟机或是使用已有的机器进行
最好是 2核2G 40G硬盘以上的机器，（需要单独为服务端本体预留40G）
## ssh/shell工具
> ### windows平台
>
> cmd powershell mobaxterm windterm tabby 等

> ### Linux平台
>
> terminal windterm 等

> ### Mac平台
> terminal

## 安装docker及其组件（以ubuntu为例）
### 为什么使用docker：

因为封装为docker即可无需顾虑linux发行版的差异，只要能顺利安装docker的linux发行版基本都可以运行（大概）
### 现在我们可以尝试安装docker

### 本格の手动安装
[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

官网说得比较清楚，可以一步一步复制到终端中进行安装


到 ```sudo docker run hello-world``` 这里就可以结束了，下面的不使用sudo运行docker（步骤可选）

### 将docker开放给其他指定用户，不使用sudo运行docker（可选）
参考官方

[https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/)

### 官方的一键安装
```shell
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

#### 假如出现了意料之外的问题，也可以先使用dry-run的方式排障
```shell
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh --dry-run
```



## 安装好docker及其组件了！拉取镜像以及启动容器
```shell
docker pull joedwards32/cs2
```

## 拉取镜像的同时打开你的浏览器，申请一个gameserver token, CS2为730
浏览器访问steam社区服务器管理页面

[https://steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers)

申请一个用于CS2服务端的token，填入下一步的docker-compose.yml配置文件中



**请找一个合理的路径放置以下文件**



```shell
# 找一个合理的路径创建名为docker-compose.yml的配置文件
vim docker-compose.yml

# 填入https://github.com/joedwards32/CS2/blob/main/examples/docker-compose.yml的内容，如果遇到网络问题，你也可以直接clone该仓库或是通过其他代理的方式下载该文件，因为配置文件会更新，这里不贴出





```
### 将docker-compose.yml中
### SRCDS_TOKEN项改为
SRCDS_TOKEN=你的token
```shell
SRCDS_TOKEN=xxxxxx
```

### volume项（持久化存储容器的数据，否则下次重新删除容器启动将会丢失所有数据！）
```yml
volumes:
      - cs2:/home/steam/cs2-dedicated/ 
```
改为
```yml
volumes:
      - ./cs2:/home/steam/cs2-dedicated/ 
```



## 海内存知己，天涯若比邻，就快大功告成了！
> 启动docker compose
```shell
docker compose up -d
```
## 更改了docker-compose.yml，如何生效？（将启动新的容器替换旧容器，请勿在对战中使用）
```docker compose up -d```

## 安装遇到问题，需要强制验证时，修改docker-compose.yml
STEAMAPPVALIDAT=1


## 查看容器的日志，连接到容器 
> ### 查看容器日志（可以依据日志判断下载是否成功，国内连接到steam网络可能有些困难，会经历多次重试）
根据docker-compose.yml中 container_name可以得知容器名为cs2-dedicated，可以使用tab进行补全
docker logs cs2-dedicated
docker logs cs2-dedicated -f

服务端会开始自动下载，请保证目标目录有40G的空间，并耐心等待
> ### 连接到容器中/服务端控制台
docker attach cs2-dedicated
> ### 断开连接
*注意* 断开连接(detach)是ctrl + p + q，不是ctrl+C，ctrl + C会终止进程

## 打开服务器的防火墙（重要，这样玩家才可以连接到服务端）
允许 27015 UDP


# 进阶篇（自定义内容）

## 安装metamod
metamod官网找到dev分支(2.x)

[https://www.metamodsource.net/downloads.php?branch=dev](https://www.metamodsource.net/downloads.php?branch=dev)

```shell
wget "https://mms.alliedmods.net/mmsdrop/2.0/mmsource-2.0.0-git1286-windows.zip"
```

解压到cs2/cs2-dedicated/game/csgo中

```shell
unzip -d cs2/cs2-dedicated/game/csgo 具体下载的文件名.zip
```
## 安装CounterStrikeSharp（首次安装必须要使用有runtime的版本）

### 源仓库
[https://github.com/roflmuffin/CounterStrikeSharp/releases](https://github.com/roflmuffin/CounterStrikeSharp/releases)

```shell
unzip -d cs2/cs2-dedicated/game/csgo 具体下载的文件名.zip
```
### 使用手册
https://busheezy.github.io/CounterStrikeSharp/docs/guides/getting-started.html
unzip -d


## 更改服务器模式
可以参考valve的官方文档
[https://developer.valvesoftware.com/wiki/Counter-Strike:_Global_Offensive/Game_Modes](https://developer.valvesoftware.com/wiki/Counter-Strike:_Global_Offensive/Game_Modes)

修改docker-compose.yml
```yml
- CS2_GAMETYPE=0 
- CS2_GAMEMODE=1 
```

## 设置你的初始地图/地图组，使用创意工坊地图/地图组
待更新
## 
