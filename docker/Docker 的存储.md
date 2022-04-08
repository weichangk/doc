虽然可以在 docker 容器中保存写入的数据，但还是有这样几个不足：

1. 容器中的数据会随着容器的停止运行而消失， 而且当其他的进程需要这些数据时，很难将这些数据从容器中提取出来；
2. 默认情况下，在运行中的容器里创建的文件，被保存在一个可写的容器层，容器的数据写入层是紧密地对应着他的宿主操作系统的，数据不能容易的被迁移到其他地方；
3. 要将数据写入到容器的数据写入层，需要一个特定的存储驱动，利用 linux 内核构建一个统一的文件系统，来管理宿主和容器的文件系统。这层额外的虚拟化显然会降低性能。为了避免性能下降，docker 使用 data volume 的方式，直接对宿主文件系统进行写操作。



Docker提供了三种不同的方式用于将宿主的数据挂载到容器中

1. Data Volume, 由Docker管理，(/var/lib/docker/volumes/ Linux), 持久化数据的最好方式
2. Bind Mount，由用户指定存储的数据具体mount在系统什么位置
3. tmpfs Mount



# 三种挂载方式了解

1. Volume方式下：容器内的数据被存放到宿主机(linux)一个特定的目录下(/var/lib/docker/volumes/)。这个目录只有Docker可以管理，其他进程不能修改。如果想持久保存容器的应用数据，Volumes是Docker推荐的挂载方式。
2. Bind mount方式下：容器内的数据被存放到宿主机文件系统的任意位置，甚至存放到一些重要的系统目录或文件中。除了Docker之外的进程也可以任意对他们进行修改；
3. tmpfs方式下：容器的数据只会存放到宿主机的内存中，不会被写到宿主机的文件系统中，因此不能持久保存容器的应用数据。





![1646882652518](D:\baocai\doc\docker\Docker 的存储\1646882652518.png)



   **Volume**

- 由Docker进程创建和管理。可以通过命令docker volume create来创建一个指定的卷，也可以由Docker进程在创建容器或服务的过程中来创建；
- 当你为一个容器创建一个volume时，这个volume将会被存储到宿主机的一个目录下。当你挂载这个volume到一个容器中时，就是在挂载这个目录到容器中。这和bind  mount的工作机制很相似，当然，除了volume是被Docker所管理并与宿主机其他核心功能隔离之外。
- 一个volume可以被同时挂载到多个容器中。当没有任何容器在使用这个volume的时候，这个volume也仍然可以被Docker进程所使用，而不是自动被删除。当然，你可以使用命令手动删除volume：docker volume prune
- 当你挂载一个volume时，可以选择为他命名(named)，也可以不命名(anonymous).  如果不命名的话，当这个volume首次被挂载到一个容器中时，Docker进程会分配给它一个随机的名字，以保证在宿主机操作系统中这个volume的名字唯一。除了名字之外，命名和不命名的volume没有区别。
- Volume也支持使用volume drivers，以帮助你将数据保存到远程主机或云上。



 **Bind mount**

- 在docker的早期版本中就存在的功能，与volumes相比，他的功能比较局限。当使用bind  mounts时，宿主机的目录或文件被挂载到容器中。容器将按照挂载目录或文件的绝对路径来使用或修改宿主机的中的数据。宿主机中的目录或文件不需要预先存在，在需要的时候会自动创建。使用Bind  mounts在性能上是非常好的，但这依赖于宿主机有一个目录妥善结构化的文件系统。如果你要创建一个新的Docker应用，我们仍推荐使用named  volume的方式，因为你无法通过Docker CLI来管理bind mounts。（警告：bind  mounts是一把双刃剑，因为使用bind  mounts的容器可以在通过容器内部的进程对主机文件系统进行修改，包括创建，修改和删除重要的系统文件和目录，这个功能虽然很强大，但显然也会造成安全方面的影响，包括影响到宿主机上Docker以外的进程）

 **tmpfs mounts**

- 在这种挂载方式下，容器内的应用数据将不会被持久的保存到硬盘上，其中的数据只能在某个容器的生存周期内被使用，或者用于保存一些不需要持久存储，或一些敏感的数据信息。比如，在docker内部，swarm服务使用tmpfs方式来将secrets挂载到服务的容器中。（关于secrets，参考https://docs.docker.com/engine/swarm/secrets/）



Bind mounts和volumes都可以通过使用标志-v或--volume来挂载到容器中，只是格式有些许不同。tmpfs可以使用标志--tmpfs进行挂载。然而，在Docker17.06及其以上版本中，我们推荐使用--mount来对容器或服务进行这三种方式的挂载，因为这种格式更加清晰。



**Volume的适用场景**

- 多个容器间需要共享数据。如果volume没有手动被创建，它将会在首次挂载到某个容器之前被自动创建，当容器被停止或删除时，这个volume不会随之被删除。多个容器可以同时以rw或ro的方式挂载这个volume。只有手动指定删除volume，它才会被删除。
- 当宿主机并没有专用于Docker的文件系统结构时。使用volume可以使宿主机的配置与容器的运行解耦。
- 当你希望将数据保存到远程主机或云上。
- 当你希望在不同的宿主机直接备份/恢复/迁移数据时，volume是一个很好的选择。你可以停止运行使用volume的容器，然后直接备份volume所在的目录即可，如/var/lib/docker/volumes/<volume-name>

**bind mounts的适用场景**

- 将宿主机的系统配置文件共享给容器，这是Docker为容器提供DNS配置的默认方式，即通过bind mounts的方式将宿主的的/etc/resolv.conf文件挂载到容器中。
- 将宿主机开发环境中的源代码或实验结果共享给容器。例如：你在宿主机上进行一个项目Maven的测试，每次你在宿主机上对Maven项目进行了更改后，容器就可以直接获取更改后的结果
- 当你可以确定宿主机的文件系统结构应该与容器内部完全一致时。

**tmpfs的适用场景**

- 当你出于安全原因，或者容器性能优化的原因（如需要写入大量的不持久的状态数据时），不需要容器的数据长久保存时可以使用这种方式。



参考：https://www.coonote.com/docker-note/docker-data-manage.html







# Data Volume

## Volume 优点

volume 是 Docker 数据持久化机制。bind mount 依赖主机目录结构，volume完全由Docker管理。Volume 有以下优点：  

- Volume 更容易备份和移植。
- 可以通过 Docker CLI 或 API 进行管理
- Volume 可以无区别的工作中 Windows 和 Linux 下。
- 多个容器共享 Volume 更安全。
- Volume 驱动可以允许你把数据存储到远程主机或者云端，并且加密数据内容，以及添加额外功能。
- 一个新的数据内容可以由容器预填充。



而且，volume不会增加容器的大小，生命周期独立与容器。如果容器不需要持久化数据，可以使用 tmpfs mount方式，可以避免容器的写入层数据写入。



## Volume 初体验测试

使用的 my-cron 就是一个 crontab 格式的计划任务，比如,每隔一分钟输出时间到一个文件里

在 linux 主机中准备一个Dockerfile 和一个 my-cron的文件

```shell
[root@VM-20-7-centos mydockertest]# mkdir hellovolume #创建测试目录
[root@VM-20-7-centos mydockertest]# cd hellovolume/ #进入测试目录
[root@VM-20-7-centos hellovolume]# touch my-cron
[root@VM-20-7-centos hellovolume]# vim my-cron 
[root@VM-20-7-centos hellovolume]# more my-cron 
*/1 * * * * date >> /app/test.txt
[root@VM-20-7-centos hellovolume]# touch hellovolume.Dockerfile #创建Dockerfile文件
[root@VM-20-7-centos hellovolume]# vim hellovolume.Dockerfile #编写Dockerfile文件
[root@VM-20-7-centos hellovolume]# cat hellovolume.Dockerfile #查看Dockerfile文件
FROM alpine:latest
RUN apk update
RUN apk --no-cache add curl
ENV SUPERCRONIC_URL=https://github.com/aptible/supercronic/releases/download/v0.1.12/supercronic-linux-amd64 \
    SUPERCRONIC=supercronic-linux-amd64 \
    SUPERCRONIC_SHA1SUM=048b95b48b708983effb2e5c935a1ef8483d9e3e
RUN curl -fsSLO "$SUPERCRONIC_URL" \
    && echo "${SUPERCRONIC_SHA1SUM}  ${SUPERCRONIC}" | sha1sum -c - \
    && chmod +x "$SUPERCRONIC" \
    && mv "$SUPERCRONIC" "/usr/local/bin/${SUPERCRONIC}" \
    && ln -s "/usr/local/bin/${SUPERCRONIC}" /usr/local/bin/supercronic
COPY my-cron /app/my-cron
WORKDIR /app

VOLUME ["/app"] # 指定挂载卷

# RUN cron job
CMD ["/usr/local/bin/supercronic", "/app/my-cron"]
[root@VM-20-7-centos hellovolume]# 

```



构建镜像

```shell
[root@VM-20-7-centos hellovolume]# docker image build -f hellovolume.Dockerfile -t hellovolume .

```

运行容器

```shell
[root@VM-20-7-centos hellovolume]# docker run -d hellovolume # 运行容器
f17f055f3a3e3998a0052dd2380a5ef4c7b03ca4ef39b9203d391733a667979b
[root@VM-20-7-centos hellovolume]# docker volume ls # 查看 volume
DRIVER    VOLUME NAME
local     0fb6ca729e0bc086c4e2903e2264a431992b8a148d035d49fa1f9296ba246506
[root@VM-20-7-centos hellovolume]# docker volume inspect 0fb6ca729e0bc086c4e2903e2264a431992b8a148d035d49fa1f9296ba246506 # 查看 volume 详细信息
[
    {
        "CreatedAt": "2022-03-10T16:58:00+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/0fb6ca729e0bc086c4e2903e2264a431992b8a148d035d49fa1f9296ba246506/_data",
        "Name": "0fb6ca729e0bc086c4e2903e2264a431992b8a148d035d49fa1f9296ba246506",
        "Options": null,
        "Scope": "local"
    }
]
[root@VM-20-7-centos hellovolume]# 

```

因为在创建容器的时候没有指定 -v 此时Docker会自动创建一个随机名字的volume，去存储我们在Dockerfile定义的volume 。

通过docker volume inspect xxx 可以查看到这个Volume的mountpoint可以发现容器创建的文件。Volume 对于的容器可以通过 docker inspect 容器id 查看容器挂载路径(Mounts：Source)找到对应Volume。



查看主机 volume 文件

```shell
[root@VM-20-7-centos /]# cd /var/lib/docker/volumes/0fb6ca729e0bc086c4e2903e2264a431992b8a148d035d49fa1f9296ba246506/_data
[root@VM-20-7-centos _data]# ls
my-cron  test.txt
[root@VM-20-7-centos _data]# cat test.txt 
Thu Mar 10 08:58:00 UTC 2022
Thu Mar 10 08:59:00 UTC 2022
Thu Mar 10 09:00:00 UTC 2022
Thu Mar 10 09:01:00 UTC 2022
Thu Mar 10 09:02:00 UTC 2022
[root@VM-20-7-centos _data]# 
```



查看主机 volume 对应容器的文件

```shell
[root@VM-20-7-centos _data]# docker exec -it f17f055f3a3e  sh
/app # ls
my-cron   test.txt
/app # cat test.txt 
Thu Mar 10 08:58:00 UTC 2022
Thu Mar 10 08:59:00 UTC 2022
Thu Mar 10 09:00:00 UTC 2022
Thu Mar 10 09:01:00 UTC 2022
Thu Mar 10 09:02:00 UTC 2022
/app # cd ..
/ # ls
app    bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var

```

###  在创建容器(指定-v参数)

在创建容器的时候通过 `-v` 参数我们可以手动的指定需要创建Volume的名字，以及对应于容器内的路径，这个路径是可以任意的，不必需要在 Dockerfile 里通过 VOLUME 定义



创建 dockerfile 不指定 volume

```shell
FROM alpine:latest
RUN apk update
RUN apk --no-cache add curl
ENV SUPERCRONIC_URL=https://github.com/aptible/supercronic/releases/download/v0.1.12/supercronic-linux-amd64 \
    SUPERCRONIC=supercronic-linux-amd64 \
    SUPERCRONIC_SHA1SUM=048b95b48b708983effb2e5c935a1ef8483d9e3e
RUN curl -fsSLO "$SUPERCRONIC_URL" \
    && echo "${SUPERCRONIC_SHA1SUM}  ${SUPERCRONIC}" | sha1sum -c - \
    && chmod +x "$SUPERCRONIC" \
    && mv "$SUPERCRONIC" "/usr/local/bin/${SUPERCRONIC}" \
    && ln -s "/usr/local/bin/${SUPERCRONIC}" /usr/local/bin/supercronic
COPY my-cron /app/my-cron
WORKDIR /app

# RUN cron job
CMD ["/usr/local/bin/supercronic", "/app/my-cron"]
[root@VM-20-7-centos hellovolume]# 

```



创建镜像

```shell
[root@VM-20-7-centos hellovolume]# docker build -f hellovolumenov.Dockerfile -t hellovolumenov .
```



创建容器时指定 volume

```shell
[root@VM-20-7-centos hellovolume]# docker run -d --name hellovolumecontainer -v hellovolume-data:/app hellovolumenov 
d68b5a246f336d738d59e388136b8e9a2759461bb1828826cd5eb04f1c11a571
[root@VM-20-7-centos hellovolume]# docker volume ls
DRIVER    VOLUME NAME
local     0fb6ca729e0bc086c4e2903e2264a431992b8a148d035d49fa1f9296ba246506
local     hellovolume-data
[root@VM-20-7-centos hellovolume]# cd /
[root@VM-20-7-centos /]# cd /var/lib/docker/volumes/hellovolume-data/
[root@VM-20-7-centos hellovolume-data]# ls
_data
[root@VM-20-7-centos hellovolume-data]# cd _data/
[root@VM-20-7-centos _data]# ls
my-cron  test.txt
[root@VM-20-7-centos _data]# cat test.txt 
Thu Mar 10 09:54:00 UTC 2022
Thu Mar 10 09:55:00 UTC 2022
Thu Mar 10 09:56:00 UTC 2022
[root@VM-20-7-centos _data]# 
```



在 Dockerfile 中指定容器数据卷的路径后，如果在创建运行容器时没有指定主机挂载路径则会生成匿名数据卷，如果指定主机挂载路径则会生产具名数据卷。Dockerfile 中可以指定容器多个数据卷（挂载目标），格式如：VOLUME ["<路径1>", "<路径2>"...]。但是好像不能在 Dockerfile 中给主机指定具名数据卷挂载（挂载源）？



# Data Volume 练习 MySQL

使用 MySQL 镜像创建运行容器未指定挂载，默认生成匿名挂载

```shell
[root@VM-20-7-centos /]# docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:latest #创建容器
07a5c37235980c563e29ef669632ff80e1c9e164e1da6b70cfdc19d7f569acc8
[root@VM-20-7-centos /]# docker ps -a
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS                    PORTS                                                  NAMES
07a5c3723598   mysql:latest     "docker-entrypoint.s…"   10 seconds ago   Up 9 seconds              0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   mysql-test
[root@VM-20-7-centos /]# docker exec -it 07a5c3723598 /bin/bash #进入容器
root@07a5c3723598:/# cd var/lib/mysql #进入mysql容器数据存储目录（之前通过数据库客户端进行创建数据库test）
root@07a5c3723598:/var/lib/mysql# ls -l #查看数据库数据文件有存在test
total 198068
drwxr-x--- 2 mysql mysql     4096 Mar 11 03:09  sys
drwxr-x--- 2 mysql mysql     4096 Mar 11 03:12  test
root@07a5c3723598:/# exit
exit
[root@VM-20-7-centos /]# docker inspect 07a5c3723598 #查看容器详细信息找到主机挂载目录
[
    {
        "Mounts": [
                    {
                        "Type": "volume",
                        "Name": "9c6e1cb9f92b37e729457b0c225adad136705f97c6717bc218c5c22f8dd5ca16",
                        "Source": "/var/lib/docker/volumes/9c6e1cb9f92b37e729457b0c225adad136705f97c6717bc218c5c22f8dd5ca16/_data",
                        "Destination": "/var/lib/mysql",
                        "Driver": "local",
                        "Mode": "",
                        "RW": true,
                        "Propagation": ""
                    }
                ],
    }
]
[root@VM-20-7-centos /]# cd /var/lib/docker/volumes/9c6e1cb9f92b37e729457b0c225adad136705f97c6717bc218c5c22f8dd5ca16/_data #进入主机挂载目录
[root@VM-20-7-centos _data]# ls -l #查看主机挂载数据发现和容器的一直
total 198068
drwxr-x--- 2 polkitd input     4096 Mar 11 11:09 sys
drwxr-x--- 2 polkitd input     4096 Mar 11 11:12 test
[root@VM-20-7-centos _data]# 

```



由于匿名挂载不方便查看查找对应挂载到那个容器，所以建议使用具名挂载。使用具名挂载在容器销毁重建的时候也方便对数据还原。

```shell
[root@VM-20-7-centos _data]# docker run -itd --name mysql-latest -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -v mysql-latest-data:/var/lib/mysql mysql:latest --lower_case_table_names=1 #指定挂载，数据库名和表名设置为对小写不敏感
9eb19dc69b1c444eacd5bd9150b62956c81467e8e176bd155f8639877f36effa
[root@VM-20-7-centos _data]# docker volume ls
DRIVER    VOLUME NAME
local     mysql-latest-data
[root@VM-20-7-centos _data]# 

```

测试，删除容器后，重新创建容器，使用之前的挂载恢复数据库容器数据

```shell
[root@VM-20-7-centos /]# docker rm -f 9eb19dc69b1c # 删除容器
9eb19dc69b1c
[root@VM-20-7-centos /]# docker run -itd --name mysql-latest -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -v mysql-latest-data:/var/lib/mysql mysql:latest --lower_case_table_names=1 #重新创建并运行容器，挂载目录还是删除之前的目录
566e3d764f8b63f5924c58ab4df35c5a8e6340e5ce0f17d40b7e4b86f980d8f3
[root@VM-20-7-centos /]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                                  NAMES
566e3d764f8b   mysql:latest   "docker-entrypoint.s…"   8 seconds ago   Up 7 seconds   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   mysql-latest
[root@VM-20-7-centos /]# docker exec -it 566e3d764f8b /bin/bash # 进入容器查看之前删除的容器的挂载数据恢复了
root@566e3d764f8b:/# cd var/lib/mysql 
root@566e3d764f8b:/var/lib/mysql# ls
'#ib_16384_0.dblwr'   auto.cnf	      binlog.index      client-key.pem	 ibdata1     performance_schema   server-key.pem   undo_002
'#ib_16384_1.dblwr'   binlog.000001   ca-key.pem        ib_buffer_pool	 ibtmp1      private_key.pem	  sys
'#innodb_temp'	      binlog.000002   ca.pem	        ib_logfile0	 mysql	     public_key.pem	  ttt
 9eb19dc69b1c.err     binlog.000003   client-cert.pem   ib_logfile1	 mysql.ibd   server-cert.pem	  undo_001
root@566e3d764f8b:/var/lib/mysql# exit
exit
[root@VM-20-7-centos /]# docker volume ls
DRIVER    VOLUME NAME
local     mysql-latest-data
[root@VM-20-7-centos /]# 

```



## volume 总结

```
在 docker run 中使用 -v 时，
	如果挂载源为具体路径时如果路径不存在会自动创建，但是如果挂载源是具体某个文件时需要确保该文件是存在的并指定文件路径
	如果使用挂载节点做为挂载源时会自动在 /var/lib/docker/volumes/ 下生成挂载目录
	如果不知道挂载源会自动在 /var/lib/docker/volumes/ 下生成匿名挂载目录

在 docker-compose 中管理挂载
	如果挂载源为具体目录路径或具体文件路径时需要指定 volumes 的 type、source、target
	services:
		db:
			volumes:
			  - type: bind
				source: D:/dockervolumes/minishopids/db
				target: /var/lib/mysql
		
	
	如果使用挂载节点做为挂载源时会自动在 /var/lib/docker/volumes/ 下生成挂载目录
	services:
		db:
			volumes:
			  - type: bind
				source: xxx
				target: /var/lib/mysql
		volumes:
			xxx:

删除所有未被容器引用的挂载卷
docker volume rm $(docker volume ls -qf dangling=true)
```



## volume 疑问（待学习）

- 怎么给正在运行的容器追加挂载卷？






# Bind Mount



# 多个机器之间的容器共享数据