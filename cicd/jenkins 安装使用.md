

# windows 环境下安装



## 安装容器并运行

```shell
docker run -d --restart=always ^
--name jenkins_01 ^
-p 8080:8080 -p 50000:50000 ^
-v D:/dockervolumes/jenkins_home:/var/jenkins_home ^
-v /var/run/docker.sock:/var/run/docker.sock ^
-v /usr/bin/docker:/usr/bin/docker ^
-v /usr/local/bin/docker-compose:/usr/local/bin/docker-compose ^
jenkins/jenkins
```



Docker Desktop on Windows 安装时是自带 docker-compose 的

为了确保构建脚本是否能使用 docker 和 docker-compose 相关命令，进入 jenkins_01 容器查看是有docker 和 docker-compose

```shell
$ docker -v
Docker version 20.10.10, build b485636
$ docker-compose -v
/bin/sh: 2: docker-compose: Permission denied
```

发现 docker-compose 没有使用权限！！！
linux 环境下修改权限方式为 chmod 777 -R /usr/bin/docker-compose:/usr/bin/docker-compose
windows 环境下怎么修改权限，懵逼了！
后来发现 docker-compose: Permission denied 不是权限问题，进入挂载目录发现 docker-compose 根本就没有挂载成功！linux环境下 docker-compose 的路径是 /usr/local/bin/docker-compose，/usr/bin/docker 能挂载成功，docker-compose 却挂载不成功！！！

还不懂怎么进不了 Docker 虚拟机文件目录，网上查找资料发现 Docker Desktop on Windows 有设置共享目录，但是安装最新版的居然没有，
有教程说在设置里切换 switch windows\linux containter 也没有能设置共享目录呀！



docker 命令可以通过挂载获得，docker-compose 可以通过通过 Dockerfile 下载 docker-compose 创建自定义 jenkins 镜像获得。

Dockerfile 文件 Dockerfile-Myjenkins 如下

```shell
FROM jenkins/jenkins  
USER root  
RUN curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \  
  && chmod +x /usr/local/bin/docker-compose
```

生成 myjenkins 镜像

```shell
docker build -t myjenkins -f Dockerfile-Myjenkins .
```

使用 myjenkins 镜像运行 jenkins_01 容器

```shell
docker run -d --restart=always ^
--name jenkins_01 ^
-p 8080:8080 -p 50000:50000 ^
-v D:/dockervolumes/jenkins_home:/var/jenkins_home ^
-v /var/run/docker.sock:/var/run/docker.sock ^
-v /usr/bin/docker:/usr/bin/docker ^
myjenkins
```

进入 jenkins_01 容器查看，终于有了是 docker 和 docker-compose 

```shell
$ docker -v
Docker version 20.10.10, build b485636
$ docker-compose -v
docker-compose version 1.29.2, build 5becea4c
```



## 访问 8080 端口获取密钥登录

```
docker logs jenkins_01
或
D:\dockervolumes\jenkins_home\secrets\initialAdminPassword
```

## 安装插件（推荐安装）

安装插件出现一个错误： No such plugin: cloudbees-folder
安装插件 cloudbees-folder 失败，是因为下载的 Jenkins.war 里没有 cloudbees-folde r插件
需要去 https://updates.jenkins-ci.org/download/plugins/cloudbees-folder/ 下载一个插件
放在 D:\dockervolumes\jenkins_home\war\META-INF

镜像源更换为国内下载会快点
D:\dockervolumes\jenkins_home\hudson.model.UpdateCenter.xml

```
https://updates.jenkins.io/update-center.json
改为
https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

## 创建管理员



# linux 环境下安装



## 创建一个 jenkins 的挂载工作目录



## 创建容器并运行

```shell
docker run \
  --restart=always \
  --name jenkins_01 \
  -d \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /usr/bin/docker:/usr/bin/docker \
  -v /usr/local/bin/docker-compose:/usr/local/bin/docker-compose \
  jenkins/jenkins
```



在宿主机中 `jenkins_home` 挂载卷，用于备份 Jenkins 中的数据。



因为会使用到容器提供环境，以及使用 Docker 打包 .NET Core 程序为 Docker 镜像，所以需要在 Jenkins 容器中映射 Docker 的 `.sock` 文件，以便在容器中，还能使用 Docker 命令。

如果不挂载 usr/bin/docker 可能报错：docker: command not found



挂载 docker-compose（挂载前确保安装了正确版本的docker-compose），确保可以在 jenkins 构建脚本中使用 docker-compose 命令，如果不挂载 usr/bin/docker 可能报错： docker-compose: command not found



进入  jenkins_01 容器查看 docker、docker-compose 是否挂载成功

```shell
docker exec -it jenkins_01 /bin/bash
jenkins@2a4964a08904:/$ docker -v
Docker version 20.10.12, build e91ed57
jenkins@2a4964a08904:/$ docker-compose -v
docker-compose version 1.29.2, build 5becea4c
```



## 修改权限

修改权限，否则会报错：jenkins  Got permission denied while trying to connect to the Docker daemon socket at unix，在宿主机执行如下命令

```shell
# 如果报权限不足，可以看需要修改以下文件权限(777 可读可写可操作，-R 表示包含所有子目录)
chmod 777 /var/run/docker.sock:/var/run/docker.sock 
chmod 777 -R /usr/bin/docker:/usr/bin/docker 
chmod 777 -R /usr/local/bin/docker-compose:/usr/local/bin/docker-compose
```





## 访问 8080 端口获取密钥登录



输入管理员密码，密码使用 `docker logs {容器ID}`  查看日志获取 Jenkins 登录密码

```
Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

19b66ff267464b76aaaaaaaaaaaaaaaa

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

```



## 安装插件（推荐安装）

如过组件安装失败可修改配置 jenkins_home/jenkins_home]# cat hudson.model.UpdateCenter.xml

```
https://updates.jenkins.io/update-center.json
https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

## 创建管理员





# 使用 jenkins 官方安装方法

jenkins 官方使用了 docker:dind 实现一种 docker in docker 的方式，实现了  sock binding。

dind 常见玩法不光有 sock binding，还有特权模式运行，以及走tcp和daemon通信两种模式。一般来说不建议简单的把宿主的sock映射到容器内部，有安全风险，不过这三种方案现阶段都有瑕疵，权衡下使用就好。适用场景：追求高度隔离，宿主环境也是容器的场景，包括并不仅限于CI/CD。

尝试使用了 docker:dind 安装 jenkins ，发现容器嵌套太深了，了解的还不够，发现网络连接和挂载上很复杂。。

参考：https://www.jenkins.io/doc/book/installing/docker/



# 任务配置

- 创建任务名 XXX 选任务类型，以Freestyle project 举例



- 源码管理配置

  仓库地址 Repository URL
  ​	https://github.com/weichangk/MiniShop.git
  ​	
  添加凭据 Credentials
  ​	Kind：选择SSH Username with private key
  ​	Username：随便填写
  ​	Private Key：选择Enter directly
  ​	Key：系统生成的git id_rsa中的Key
  ​	
  凭据保存后返回源码管理 Credentials：选择刚才填写的 UserName


- 指定分支

  ​	*/master（看具体而定）

  ​	

- 添加构建步骤

  ​	本案例选择shell，输入 shell 脚本

  ​	

- 点击保存，重新构建Job。

- 如果使用 docker-compose 结合 .env 环境变量配置进行部署，在生产环境服务器中只要将 prod.env 环境变量配置文件放置 jenkins 项目构建任务工作空间目录如：/var/lib/docker/volumes/jenkins_home/_data/workspace/minishopids，

  在启动容器时指定 prod.env 文件如：docker-compose -f docker-compose.yml -p minishopids --env-file prod.env up --detach 即可切换生产环境配置。

- 









