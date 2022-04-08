先看下能不能访问 github

```
ping github.com
```



如不能访问需要配置 host，访问网址：https://www.ipaddress.com/，查找github最新的ip地址，进行hosts配置

```she&#39;l&#39;l
140.82.114.4 github.com
199.232.69.194 github.global.ssl.fastly.net
```

重启host

```she&#39;l&#39;l
/etc/init.d/network restart
# ipconfig/flushdns # windows 
```



拉取代码时报错

```
fatal: remote error: 
  The unauthenticated git protocol on port 9418 is no longer supported.
Please see https://github.blog/2021-09-01-improving-git-protocol-security-github/ for more information.
```



需要手动关闭ssl验证

```shell
git config --global http.sslverify false
```



提交代码需要配置账号信息

```shell
git config --global user.name "xxx"
git config --global user.email "xxx@gmail.com"
```

配置ssh-key私钥

添加ssh-key私钥到  ssh-agent，首先确保ssh-agent 正常工作，结果类似：Agent pid 94457 即为正常

```shell
eval $(ssh-agent -s)
```



登录 GitHub 找到 setting SSH 配置复制私钥到 /root/.ssh/id_rsa.pub

```shell
vim /root/.ssh/id_rsa.pub
```

