# 一、虚拟机网络适配器设置

## 1.1 NAT模式 （可以上网）

​	虚拟机默认为nat模式，当选用nat模式时，虚拟机的网段跟VMnat8 处于同一个网段。

<img src="S:\Typora_BiJi\Kali\imgs\1.1.1.png" style="zoom: 50%;" />



## 1.2 桥接模式 （不可以上网）

​	若虚拟机实用桥接模式，虚拟机网段跟 物理机WLAN网段一致。

<img src="S:\Typora_BiJi\Kali\imgs\1.2.1.png" style="zoom:50%;" />



## 1.3 仅主机模式 （不可以上网，完全隔离）

​	若虚拟机使用，仅主机模式，虚拟机将不能上网，并且虚拟机和物理机的 ==VWnat1== 属于同一个网段。

<img src="S:\Typora_BiJi\Kali\imgs\1.3.1.png" style="zoom:50%;" />



## 1.4 若桥接模式上不了网

​	查看网络编制是否正确

<img src="S:\Typora_BiJi\Kali\imgs\1.4.1.png" style="zoom: 80%;" />



# 二、 apt

## 2.1 apt换源流程

### 2.1.1 apt换源

​	首先进行换源，在终端中输入 `mousepad /etc/apt/sources.list`,然后将准备好的源镜像copy到文件中，保存退出即可。

<img src="S:\Typora_BiJi\Kali\imgs\2.1.1.png" style="zoom:55%;" />

### 2.1.2 apt应用列表更新

​	终端中输入 `apt update`等待即可。

<img src="S:\Typora_BiJi\Kali\imgs\2.1.2.png" style="zoom:60%;" />

> [!WARNING]
>
> 切记不要使用 `apt upgrade`



## 2.2.1 apt搜索

​	`apt search + 信息`可以在apt中查询相关内容



# 三、 SSH远程控制



## 3.1 启动SSH服务

​	在虚拟机终端中， `systemctl start SSH`开启SSH服务



## 3.2 连接Kali

​	在主设备终端中输入， `SSH kali@ip地址`进行连接该虚拟机，进行远程控制。



# 四、 容器虚拟化Docker



## 4.1 Docker是什么



> Docker是一个开源的应用容器引擎，是一个用于构建，传送，运行应用程序的平台。



## 4.2 安装Docker

​	

​	安装Docker需要root用户执行。安装命令如下：

> apt install docker.io

​	

### 4.2.2 启动Docker服务

​	

​	启动docker服务：

> systemctl start docker

​	关闭docker服务：

> systemctl stop docker.socket
>
> systemctl stop docker.service



![](S:\Typora_BiJi\Kali\imgs\4.2.2.png)



## 4.3 Docker使用



### 4.3.1 Docker两大概念



> 1. 镜像 -images 应用程序的静态文件，类似虚拟机的系统镜像
> 2. 容器 -container 运行状态的应用程序，类似安装好的虚拟机

​	默认情况下，镜像与容器，都是空的

​	查看镜像：`docker images`

​	查看容器：`docker ps -a`

![](S:\Typora_BiJi\Kali\imgs\4.3.1.png)



### 4.3.2 下载镜像



​	终端输入 `docker pull 镜像名称`

​	例如：`docker pull alpine` -- alpine是一个大小为5MB的镜像



​	可以看到当拉取成功时，查看docker镜像，会在其中显示出来。

<img src="S:\Typora_BiJi\Kali\imgs\4.3.2.png" style="zoom:50%;" />

### 4.3.3 Docker容器换源

​	

​	若直接使用docker下载镜像会非常慢，修改容器镜像加快速度。这里使用阿里容器镜像服务。
​	在阿里官方打开容器，并选择 ==容器镜像服务==

<img src="S:\Typora_BiJi\Kali\imgs\4.3.3.png" style="zoom:40%;" />

​	然后跟据操作文档进行设置即可。

<img src="S:\Typora_BiJi\Kali\imgs\4.3.3b.png" style="zoom:40%;" />

> 使用 `docker info`可以查看是否添加成功的信息。如下是已经添加成功。
>
> ![](S:\Typora_BiJi\Kali\imgs\4.3.3c.png)

> [!IMPORTANT]
>
> 若阿里镜像不能使用，可以更换其他镜像源
>
> ```linux
> {
> "registry-mirrors": ["https://docker.1panel.live"]
> }
> ```
>
> 



## 4.4 Dvwa下载以及使用



### 4.4.1

​	使用 `docker pull sagikazarmark/dvwa`下载镜像

<img src="S:\Typora_BiJi\Kali\imgs\4.4.1.png" style="zoom: 33%;" />

​	`docker run -dit --name=dvwa -p 10000:80 sagikazarmark/dvwa`对dvwa端口进行更改

<img src="S:\Typora_BiJi\Kali\imgs\4.4.1a.png" style="zoom:150%;" />



## 4.5 github加速器



> https://gitclone.com



















