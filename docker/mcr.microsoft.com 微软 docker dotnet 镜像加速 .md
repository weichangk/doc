2018 年五月之后，微软将后续发布的所有 docker image 都推送到了 MCR （Miscrosoft Container Registry），但在中国，它的速度实在很慢，有时根本下载不了，下面介绍使用使用 docker-mcr 下载。



MCR(Miscrosoft Container Registry) 加速器，助你在中国大陆急速下载 netcore 相关的 docker 镜像。



docker-mcr 是一个 dotnet core global tool，简单几步，便可以进行安装和使用。

[进入dotnet页面，下载并安装 netcore 3.1 SDK](https://link.zhihu.com/?target=https%3A//dotnet.microsoft.com/download)。



安装 newbe.mcrmirror

```she&#39;l&#39;l
dotnet tool install newbe.mcrmirror -g
```

如果您曾经安装过 newbe.mcrmirror ,您需要使用以下命令来进行升级，确保最佳的体验。

```shell
dotnet tool update newbe.mcrmirror -g
```



执行 `docker-mcr -i mcr.microsoft.com` 获取配置 `config.json`文件，包含了 所有 sdk 路径和版本信息

使用 `docker-mcr -i xxx` 根据镜像路径拉取镜像。



拉取国内服务器上的镜像。docker-mcr 加速的本质是镜像推送到了国内的服务器，目前在以下服务器均存在镜像:

- 阿里云 registry.cn-hangzhou.aliyuncs.com/newbe36524
- 腾讯云 ccr.ccs.tencentyun.com/mcr_newbe36524



以下以阿里云为例进行说明，假设需要拉取 mcr.microsoft.com/dotnet/aspnet:3.1-buster-slim 打开配置文件`config.json`搜索 mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim 会找到以下节点

```
{
  "tag": "aspnet:3.1-buster-slim",
  "source": "mcr.microsoft.com/dotnet/aspnet:3.1-buster-slim"
}
```

则说明在国内镜像的 tag 为 aspnet:3.1-buster-slim。则拼接上面的前缀，则得到地址 registry.cn-hangzhou.aliyuncs.com/newbe36524/aspnet:3.1-buster-slim



参考：https://github.com/newbe36524/Newbe.McrMirror

