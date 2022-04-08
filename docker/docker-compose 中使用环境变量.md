`Docker-Compose` 文件可能会暴露配置隐私到到代码仓库，可使用环境变量指定生产环境配置。

`Docker-Compose` 环境变量参考：https://docs.docker.com/compose/environment-variables/



生产环境部署只要将生产环境变量配置文件 `prod.env` 取代开发配置即可。

在生产环境服务器中使用 `jenkins` 部署时，将生产环境变量配置文件 `prod.env` 放置项目构建任务工作空间目录如：`/var/lib/docker/volumes/jenkins_home/_data/workspace/minishopids`



`Docker-Compose` 构建命令指定  `--env-file prod.env`

如：`docker-compose -f docker-compose.yml -p minishopids --env-file prod.env up --detach`



`.env` 文件

```shell
DB_PORT=3326
DB_VOLUME_SOURCE=D:/dockervolumes/minishopids/db
DB_PASSWORD=123456
WEB_PORT=5001
WEB_VOLUME_APPSETTINGS_SOURCE=D:/dockervolumes/minishopids/appsettings.json
WEB_VOLUME_LOGS_SOURCE=D:/dockervolumes/minishopids/log
```



`Docker-Compose` 文件

```shell
version: '3.8'

services:
  db:
    image: "mysql:8.0.0"
    ports:
      - ${DB_PORT}:3306
    volumes:
      - type: bind
        source: ${DB_VOLUME_SOURCE}
        target: /var/lib/mysql
    networks:
      - backend
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
      BIND-ADDRESS: 0.0.0.0

  web:
    build: 
      context: .
      dockerfile: Dockerfile-Ids
    ports:
      - ${WEB_PORT}:80
    networks:
      - backend
    volumes:
      - type: bind
        source: ${WEB_VOLUME_APPSETTINGS_SOURCE}
        target: /app/appsettings.json
      - type: bind
        source: ${WEB_VOLUME_LOGS_SOURCE}
        target: /app/log
    environment:
      INITDB: initdb
      CONNECTIONSTRING: server=db;database=MiniShopIdsDB;uid=root;pwd=${DB_PASSWORD};SslMode=None;ConnectionReset=false;connect timeout=3600
    depends_on:
      - db

networks:
  backend:


```

