# windows 下安装

安装前准备

- 因为要开启虚拟化服务，所以使用windows操作系统至少要windows10专业版，在windows操作系统中虚拟化服务叫Hyper-V，在安装和运行dockre之前要确保windows功能Hyper-V是开启状态。

- 如果安装后提示需要 WSL2 Linux 内核更新。就点击链接（[https://docs.microsoft.com/zh-cn/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package）下载最新包：适用于](https://docs.microsoft.com/zh-cn/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package%EF%BC%89%E4%B8%8B%E8%BD%BD%E6%9C%80%E6%96%B0%E5%8C%85%EF%BC%9A%E9%80%82%E7%94%A8%E4%BA%8E) x64 计算机的 WSL2 Linux 内核更新包，安装更新包后，restart docker即可。



官网下载 Docker Desktop Installer.exe 傻瓜式安装

- 参考：https://docs.docker.com/desktop/windows/install/



配置镜像加速器

```
路径 C:\Users\xxxx\.docker\daemon.json
添加配置 "registry-mirrors":["https://pee6w651.mirror.aliyuncs.com"]
	
Docker 官方中国区：https://registry.docker-cn.com
网易：http://hub-mirror.c.163.com
中国科技大学：https://docker.mirrors.ustc.edu.cn
阿里云：https://pee6w651.mirror.aliyuncs.com
```



# linux 下安装

## 卸载旧版本

卸载它们以及相关的依赖项。

```she&#39;l&#39;l
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```



## 安装软件包

安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。

```shell
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```



## 设置镜像仓库

官方源地址

```shell
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

阿里云

```shell
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

清华大学源

```shell
yum-config-manager \
    --add-repo \
    https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
```

## 安装 Docker Engine-Community

安装前更新 yum 软件包索引

```shell
yum makecache fast
```

安装最新版本的 Docker Engine-Community 和 containerd

```shell
yum install docker-ce docker-ce-cli containerd.io
```

## 启动 Docker

```shell
systemctl start docker
```

测试Docker

```shell
[root@VM-20-7-centos /]# docker --version
Docker version 20.10.12, build e91ed57
[root@VM-20-7-centos /]# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
```



## 卸载 Docker

```shell
# 删除安装包
yum remove docker-ce
# 删除镜像、容器、配置文件等内容
rm -rf /var/lib/docker
```



## 配置镜像加速器

```
Docker 官方中国区：https://registry.docker-cn.com
网易：http://hub-mirror.c.163.com
中国科技大学：https://docker.mirrors.ustc.edu.cn
阿里云：https://pee6w651.mirror.aliyuncs.com
```

修改/etc/docker/daemon.json文件

```shell
# 1
mkdir -p /etc/docker
# 2
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
EOF
# 3
systemctl daemon-reload
# 4
systemctl restart docker
```

重启

```
systemctl daemon-reload 
systemctl restart docker
```



## 详细教程参考

Docker Docs 官网 https://docs.docker.com/engine/install/centos/

菜鸟教程 https://www.runoob.com/docker/centos-docker-install.html